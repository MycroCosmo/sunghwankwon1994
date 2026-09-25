# 권성환 | Backend Developer

Java·Spring과 PostgreSQL을 중심으로 백엔드를 개발합니다. SQL 조회 성능, 데이터 정합성, 인증·권한, 실패 처리에 관심이 있습니다. 개인 프로젝트에서는 TypeScript·NestJS와 웹·모바일 클라이언트도 다루고 있습니다.

## 주요 기술

| 구분 | 사용 기술 |
| --- | --- |
| 백엔드 | Java 17/21, Spring Boot, MyBatis, JPA, Spring Security |
| 데이터베이스 | PostgreSQL, SQL |
| 프로젝트 확장 | TypeScript, NestJS, Prisma, Nuxt 3, Flutter |
| 테스트 | JUnit, Mockito, Jest, Playwright |
| 저장소·서비스 | GCS, Supabase |

## 주요 프로젝트

### MyBatis Easy Sync Starter

기본 CRUD 작성과 Mapper/XML 불일치 확인을 돕는 Java 라이브러리입니다. 사용자 XML을 우선하고 런타임 SQL 병합과 컴파일 시점 검증을 분리했습니다. 설치에 필요한 활성화 설정과 XML 리소스 조건은 저장소 문서에 정리했습니다.

[코드와 사용 안내](https://github.com/MycroCosmo/mybatis-easy-sync-starter)

### LOCO

Flutter·NestJS·PostgreSQL/Prisma로 재구현한 장소 공유 서비스입니다. 조건부 UPDATE를 이용한 리프레시 토큰의 일회 소비, 지도방 권한 구분, 지도 영역별 마커 조회를 다뤘습니다.

별도 개선 브랜치에는 탈퇴 계정의 기존 토큰 접근 차단과 트랜잭션 기반 토큰 폐기를 구현했습니다. 해당 브랜치는 GitHub Actions에서 단위 테스트 9개, PostgreSQL API 테스트 6개와 서버 빌드를 통과했습니다. 기본 브랜치·개선 브랜치·실제 배포 상태는 구분해서 관리합니다.

소스는 비공개로 유지하고, 공개 문서에는 설계와 검증 범위를 정리했습니다.

[LOCO 설계·검증 사례](projects/LOCO.md)

### Dev Blackbox

개발 명령과 네트워크 실패를 로컬에서 기록하고 구조화된 오류 기록과 Markdown 보고서로 남기는 도구입니다. 명령 실행 기록, 로컬 수집기, 민감정보 마스킹, 데이터 보존 정책과 MCP 연결을 다룹니다.

[저장소](https://github.com/MycroCosmo/blackbox) · [한국어 사용 안내](https://github.com/MycroCosmo/blackbox/blob/main/README.ko.md)

## 실험 프로젝트

### Meetly

Nuxt 3와 Supabase로 시간 조율·장소 선택·비용 분담 흐름을 구현한 프로젝트입니다. 익명 참여 토큰, Edge Functions와 RLS를 다루며, 직접 방 수정·삭제의 권한 제한을 별도 회귀 테스트와 함께 개선하고 있습니다. 전체 운영 권한 체계가 검증 완료된 상태로 소개하지 않습니다.

[코드와 현재 범위](https://github.com/MycroCosmo/meetly)

### GameBox

Next.js와 Socket.io 기반 파티게임 프로토타입입니다. 메모리 기반 방 관리와 라이어 제시어 전달을 구현했습니다. 미션 확인·투표 판정은 아직 stub이며, 완성된 게임 서비스나 재시작 복구가 보장되는 시스템은 아닙니다.

[코드와 구현 범위](https://github.com/MycroCosmo/game-box)

## 개발 기준

문제의 원인과 적용 범위를 먼저 확인하고, 변경은 재현 가능한 테스트로 검증하려고 합니다. 문서에서는 구현된 기능, 테스트한 조건, 남은 제한을 구분합니다.

## 연락처

- 이메일: sunghwan.ian.kwon@gmail.com
- GitHub: [MycroCosmo](https://github.com/MycroCosmo)
