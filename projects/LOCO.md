# LOCO | 장소 공유 서비스 재구현

> 소스 코드는 비공개 저장소에서 관리하고 있으며, 이 문서는 채용 검토를 위한 기술 설계 및 구현 요약입니다.

## 프로젝트 개요

기존 React + Spring Boot/JPA 기반 프로젝트를 이후 **Flutter + NestJS + Prisma + PostgreSQL** 구조로 개인 재구현했습니다.

서비스 기능 자체보다 다음 백엔드 문제를 다시 설계하는 데 초점을 맞췄습니다.

- Refresh Token 재사용과 동시 요청
- 회원 탈퇴 이후 기존 세션 처리
- 공개/비공개 지도방의 읽기·쓰기 권한
- 장소 목록과 지도 Marker 조회 방식 분리
- 파일 저장소와 DB가 동시에 실패할 수 있는 상황
- 지도에 장소가 많아졌을 때 응답 크기 증가

## 기술 스택

| 영역 | 기술 |
|---|---|
| Mobile | Flutter |
| Backend | NestJS, TypeScript |
| Database | PostgreSQL |
| ORM | Prisma |
| Auth | JWT, bcrypt, Kakao/Naver OAuth |
| Map | Kakao API |
| Image | Sharp, WebP |
| Notification | Firebase Cloud Messaging |

---

## 1. Refresh Token을 한 번만 사용할 수 있게 처리

### 문제

Refresh Token을 단순히 조회한 뒤 폐기하고 새 token을 발급하면 같은 token으로 거의 동시에 두 요청이 들어오는 상황에서 둘 다 검증을 통과할 수 있습니다.

### 처리

Refresh Token 원문 대신 hash를 저장하고, 재발급 시 기존 token을 다음 조건으로 갱신합니다.

```text
WHERE id = ? AND revokedAt IS NULL
```

UPDATE 결과가 정확히 1건인 요청만 다음 Refresh Token을 만들 수 있도록 했습니다.

기존 token 폐기와 새로운 token 생성은 하나의 transaction으로 묶었습니다.

### 의도

```text
요청 A ─┐
        ├─ 같은 Refresh Token
요청 B ─┘

A: revokedAt IS NULL UPDATE → 성공
B: revokedAt IS NULL UPDATE → 0건

→ A만 다음 token 발급
```

token의 유효 여부를 애플리케이션 메모리 상태가 아니라 DB 상태 변경 결과로 결정하도록 했습니다.

---

## 2. 회원 상태와 Session 폐기

회원 탈퇴나 비밀번호 변경 이후에도 기존 Refresh Token이 살아 있으면 새로운 Access Token을 계속 발급할 수 있습니다.

따라서 계정 상태 변경 시 해당 사용자의 활성 Refresh Token을 함께 폐기합니다.

또한 Access Token으로 접근할 때도 현재 사용자 상태를 다시 확인해 탈퇴 사용자의 접근을 차단하는 방향으로 처리했습니다.

---

## 3. 공개 여부와 권한을 별도 개념으로 처리

지도방에는 다음 상태가 존재합니다.

- PUBLIC
- PRIVATE
- owner
- member
- 일반 사용자

단순히 `PUBLIC이면 접근 가능` 같은 규칙 하나로 처리하지 않고 **조회와 수정 권한을 분리**했습니다.

예:

| 동작 | 조건 |
|---|---|
| 공개 지도방 조회 | 공개 조회 정책 |
| 비공개 지도방 조회 | owner 또는 member |
| 장소 추가 | 가입 사용자 + 지도방 정책 |
| 장소 수정 | 작성자 |
| 지도방 수정/삭제 | owner |
| 비공개방 가입 | 유효한 invite code |

읽을 수 있다는 이유만으로 수정할 수 있는 구조가 되지 않도록 했습니다.

---

## 4. 장소 목록과 지도 Marker API 분리

### 문제

일반적인 장소 목록과 지도 Marker는 요구사항이 다릅니다.

장소 목록에서는 제목, 리뷰, 작성자 등의 데이터가 필요하지만 지도 화면에서는 현재 화면 영역 안의 좌표가 우선입니다.

모든 장소 데이터를 동일 API로 반환하면 지도 이동 시 불필요한 응답량이 커질 수 있습니다.

### 처리

**장소 목록**

- cursor pagination
- 검색
- tag 필터
- 정렬
- 페이지 내 장소의 리뷰 통계 묶음 조회

**지도 Marker**

- viewport bounds 기준 조회
- zoom level 반영
- 일정 개수를 넘으면 cluster 응답으로 전환
- 여러 precision의 `PlaceMarkerCell`을 이용해 집계

지도 확대 수준에 따라 필요한 데이터 크기를 다르게 가져가도록 했습니다.

---

## 5. 리뷰 통계를 N번 조회하지 않도록 처리

장소 목록에서 각 장소마다 평균 별점과 리뷰 수를 개별 조회하면 장소 개수만큼 추가 쿼리가 발생할 수 있습니다.

페이지 단위 장소를 먼저 조회한 뒤 해당 장소들의 리뷰 통계를 묶어서 가져와 응답에 결합하는 방식으로 처리했습니다.

---

## 6. 파일과 DB의 Transaction 불일치 처리

이미지 파일 저장소와 PostgreSQL은 하나의 DB transaction으로 묶을 수 없습니다.

예를 들어:

```text
새 이미지 저장 성공
        ↓
DB UPDATE 실패
```

하면 새 파일이 사용되지 않은 채 남을 수 있습니다.

반대로 기존 파일을 먼저 삭제한 뒤 DB 작업이 실패하면 사용자 데이터가 참조하던 이미지를 잃을 수 있습니다.

그래서 순서를 구분했습니다.

### 신규 파일

```text
파일 저장
  ↓
DB 변경
  ├─ 성공 → 유지
  └─ 실패 → 신규 파일 삭제
```

### 기존 파일 교체

기존 파일은 DB 변경이 성공한 뒤 삭제하도록 처리했습니다.

DB transaction과 외부 저장소를 완전한 하나의 transaction으로 만들 수 없기 때문에 **보상 동작의 실행 시점**을 명시적으로 나눴습니다.

---

## 주요 데이터 제약

Prisma schema에서 다음과 같은 중복을 DB 레벨에서도 제한합니다.

- 지도방 멤버: `(mapId, userId)`
- 장소 리뷰: `(placeId, userId)`
- 즐겨찾기: `(userId, mapId)`
- Refresh Token hash: unique
- Invite Code: unique

애플리케이션 로직만으로 중복을 막기보다 DB constraint도 함께 사용했습니다.

---

## 이 프로젝트에서 중점적으로 다룬 것

- 인증 상태 전이
- 동시 요청에서의 token 일회 소비
- 트랜잭션 경계
- DB와 외부 파일 저장소의 실패 보상
- 권한 규칙 분리
- cursor pagination
- 지도 viewport 기반 조회
- 데이터가 증가할 때의 Marker 집계 전략

소스 저장소는 현재 private으로 유지하고 있습니다.
