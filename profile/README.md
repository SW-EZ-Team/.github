# SW-EZ-Team

Java · Rust · Python 백엔드 3종과 React 프론트엔드를 함께 운영하는 폴리글랏 MSA 팀이다.

## 레포지토리 & 기술 스택

| 레포 | 역할 | 기술 스택 |
|---|---|---|
| [frontend-web](https://github.com/SW-EZ-Team/frontend-web) | 웹 프론트엔드 (SPA) | Vite + React + TypeScript, Vercel 배포 |
| [backend-spring](https://github.com/SW-EZ-Team/backend-spring) | JVM 기반 API 서버 | Java 21, Spring Boot 3.4.x, Spring Data JPA (Hibernate), Gradle Groovy DSL |
| [backend-rust](https://github.com/SW-EZ-Team/backend-rust) | 파이썬 래핑된 라이브러리 모듈 | Rust  |
| [backend-fastapi](https://github.com/SW-EZ-Team/backend-fastapi) | Python 비동기 API + 작업 큐 | Python 3.12, FastAPI, SQLAlchemy 2.0 async, uv, Celery (worker + beat) |
| [infra](https://github.com/SW-EZ-Team/infra) | 공용 인프라 + 스키마 중앙 관리 | Postgres 17, Redis 7.4, Qdrant v1.12.x, Flyway 마이그레이션 |
| [.github](https://github.com/SW-EZ-Team/.github) | 조직 프로필 | GitHub Organization Profile |

## 공용 인프라

백엔드 3종은 `infra` 레포가 띄우는 공용 Docker 네트워크 `sw-ez-shared`에 조인해서 같은 Postgres · Redis · Qdrant를 바라본다. DB 스키마는 Flyway로 `infra` 레포에서 중앙 관리하고, 각 백엔드는 자기 ORM(JPA · sqlx · SQLAlchemy)으로 매핑만 한다.

## 배포

백엔드 3종은 Docker 멀티스테이지 빌드로 통일하고, `frontend-web`은 Vercel에 바로 배포한다.
