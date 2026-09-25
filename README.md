# 권성환 | Backend Developer

Java/Spring과 PostgreSQL을 중심으로 백엔드 개발을 하고 있습니다.

단순히 기능을 구현하는 것보다 **SQL 성능, 데이터 정합성, 트랜잭션 경계, 실패 상황과 운영 시점의 예외**를 명확하게 다루는 데 관심이 있습니다.  
개인 프로젝트에서는 TypeScript/NestJS, Nuxt 3, Flutter 등도 사용하며 서비스 전체 흐름을 직접 구현하고 검증하고 있습니다.

## 주요 기술

- **Backend**: Java 17/21, Spring Boot, Spring Security, MyBatis, JPA/Hibernate
- **Database**: PostgreSQL, SQL
- **Also**: TypeScript, NestJS, Prisma, Nuxt 3, Flutter
- **Test**: JUnit, Mockito, Playwright, 회귀 테스트
- **Infra / Storage**: GCS, Supabase, Vercel

## 대표 프로젝트

### MyBatis Easy Sync Starter
MyBatis의 SQL 제어권은 유지하면서 반복되는 CRUD와 Mapper/XML 불일치 문제를 줄이기 위해 만든 Java 17 기반 라이브러리입니다.

- 사용자 XML을 자동 생성 SQL보다 우선
- Runtime CRUD와 Compile-time 검증 분리
- Annotation Processor로 Mapper/XML 불일치 감지
- 파괴적인 코드 자동수정을 피하고 개발자 판단을 남기는 방식으로 설계

https://github.com/MycroCosmo/mybatis-easy-sync-starter

### LOCO | 비공개 프로젝트 Case Study
Flutter + NestJS + PostgreSQL/Prisma로 재구현한 장소 공유 서비스입니다.

소스 저장소는 비공개로 유지하되, 인증 상태·권한·지도 조회·실패 처리 설계를 별도 문서로 정리했습니다.

- Refresh Token 일회 소비와 동시 요청 처리
- 회원 상태 변경 시 Session 폐기
- 공개/비공개 지도방 권한 분리
- viewport/zoom 기반 Marker 조회
- DB와 파일 저장소 간 실패 보상 처리

[LOCO 기술 Case Study 보기](./projects/LOCO.md)

### Dev Blackbox
AI 코딩 에이전트가 개발 중 발생시킨 오류와 네트워크 실패를 로컬에서 기록하고, 구조화된 incident와 보고서로 남기는 개발 도구입니다.

- CLI 기반 개발 명령 기록
- 로컬 network collector
- 실패 incident 중복 제거 및 Markdown 보고서 생성
- 민감정보 마스킹과 retention 정책
- MCP 연동 및 agent workflow 지원

https://github.com/MycroCosmo/blackbox

### Meetly
회원가입 없이 여러 사람이 가능 시간, 장소 투표, 비용 분담을 한 번에 정리할 수 있도록 만든 일정 조율 서비스입니다.

- Nuxt 3 + Supabase PostgreSQL
- PostgreSQL RLS 기반 권한 제어
- 30분 단위 일정 겹침 계산
- TTL 기반 임시 데이터 정리

https://github.com/MycroCosmo/meetly

### GameBox
여러 사용자가 같은 방에서 상태를 공유하는 실시간 파티게임 플랫폼입니다.

- Next.js + Node.js + Socket.io
- 서버 기준 게임 상태 관리
- Room / Game phase 모델링
- 연결 해제·재접속·상태 불일치 문제를 별도 실패 케이스로 처리

https://github.com/MycroCosmo/game-box

## 개발할 때 중요하게 보는 것

- 병목을 확인한 뒤 최적화할 것
- 상태 변화와 트랜잭션 경계를 명확하게 둘 것
- 실패, 재시도, 중복 실행, 권한 문제를 정상적인 설계 대상에 포함할 것
- 자동화나 AI가 만든 결과도 직접 검증할 수 있는 구조를 만들 것

## Contact

- Email: sunghwan.ian.kwon@gmail.com
- GitHub: https://github.com/MycroCosmo
