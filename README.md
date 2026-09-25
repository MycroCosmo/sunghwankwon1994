# Kwon Sunghwan

Backend Developer focused on **reliable APIs, SQL performance, data consistency, and failure handling**.

I mainly work with Java/Spring and PostgreSQL, and I also build product-oriented side projects with TypeScript/NestJS and Flutter. I prefer to make state transitions and failure cases explicit, then verify them with tests or reproducible scenarios.

## Core stack

- **Backend:** Java 17/21, Spring Boot, Spring Security, MyBatis, JPA/Hibernate
- **Database:** PostgreSQL, SQL tuning
- **Also used:** TypeScript, NestJS, Prisma, Nuxt 3, Flutter
- **Testing:** JUnit, Mockito, Playwright, regression testing
- **Infrastructure / Storage:** GCS, Supabase, Vercel

## Featured work

### MyBatis Easy Sync Starter
A Java 17 library for reducing repetitive MyBatis CRUD and Mapper XML work without taking SQL control away from the developer.

- Runtime CRUD SQL generation while preserving user-defined XML statements
- Naming strategy and annotation-based table/column mapping
- Compile-time Mapper/XML consistency checks with an annotation processor
- Safe generation policy that avoids destructively rewriting existing SQL

Repository: https://github.com/MycroCosmo/mybatis-easy-sync-starter

### LOCO
A map-based place sharing service rebuilt as a Flutter + NestJS application.

- Single-use refresh-token rotation backed by transactional state changes
- Public/private map membership and authorization rules
- Cursor-based place queries and viewport/zoom-aware marker retrieval
- PostgreSQL/Prisma data model with reviews, favorites, invitations, and notifications

Repository: https://github.com/MycroCosmo/loco

### Meetly
A lightweight group scheduling service that combines time availability, place voting, and expense splitting.

- Anonymous participation without mandatory signup
- PostgreSQL Row Level Security for access control
- 30-minute availability overlap calculation
- TTL-based cleanup for temporary meeting data

Repository: https://github.com/MycroCosmo/meetly

### GameBox
A real-time party game platform built with Next.js and Socket.io.

- Room and game-state synchronization over WebSocket
- Server-side game engine for phase and role transitions
- Reconnection and state synchronization treated as explicit failure cases

Repository: https://github.com/MycroCosmo/game-box

## Engineering principles

- Optimize after identifying the actual bottleneck.
- Prefer explicit transaction and state boundaries over implicit behavior.
- Treat failure, retry, duplicate execution, and authorization as normal design cases.
- Keep abstractions small enough that their behavior can be explained and tested.

## Contact

- Email: sunghwan.ian.kwon@gmail.com
- GitHub: https://github.com/MycroCosmo
