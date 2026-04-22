# SW-EZ-Team

Java · Rust · Python 백엔드와 React 프론트엔드를 함께 운영하는 폴리글랏 풀스택 팀이다. AI 기능은 FastAPI 서버가 전담하고, Rust 는 PyO3 wheel 로 FastAPI 에 in-process 임베드된다.

## 레포지토리

| 레포 | 역할 | 기술 스택 |
|---|---|---|
| [frontend-web](https://github.com/SW-EZ-Team/frontend-web) | SPA 클라이언트 | Vite 8, React 18.3.1, TypeScript |
| [backend-spring](https://github.com/SW-EZ-Team/backend-spring) | 메인 백엔드 — REST, 인증, 결제, 세션 | Spring Boot 3.4.5, Java 21, Gradle |
| [backend-fastapi](https://github.com/SW-EZ-Team/backend-fastapi) | AI · 에이전트 전담 서버 | FastAPI 0.115.5, Python 3.12, uv |
| [lib-rust](https://github.com/SW-EZ-Team/lib-rust) | OCR 전처리 PyO3 wheel — PDF 래스터화 + 이미지 전처리 (FastAPI in-process import) | Rust, PyO3, pdfium-render, image |
| [infra](https://github.com/SW-EZ-Team/infra) | 로컬 개발용 공용 인프라 + 스키마 중앙 관리 | docker-compose, PostgreSQL 18.3, Redis 8.6.2, Qdrant, Flyway |
| [.github](https://github.com/SW-EZ-Team/.github) | 조직 프로필 / 공용 템플릿 | (메타) |

## 아키텍처 개요

```
Client (React)
   │
   ▼
backend-spring  (REST · 인증 · 결제 · 세션)
   │
   ▼
backend-fastapi  (AI · 에이전트)
   ├─ import lib_rust  (PyO3 wheel — PDF → 이미지 전처리, in-process)
   └─ AI 파이프라인  (GPU 서버, Qdrant RAG 등)
```

## 공용 인프라

백엔드 서버들은 `infra` 레포가 생성하는 Docker 공유 네트워크 `sw-ez-shared` 에 조인한다. 각 백엔드 `docker-compose` 는 이 네트워크를 `external` 로 참조한다.

DB 스키마는 Flyway 가 `infra/schema/migrations/V*.sql` 에서 중앙 관리하는 것이 진실 공급원이다. 각 백엔드는 ORM 으로 매핑만 하며, Spring 의 `ddl-auto` 와 FastAPI 의 Alembic 자동 생성은 모두 비활성화 상태다.

## 배포

- **로컬 개발**: `infra` 의 docker-compose 로 Postgres · Redis · Qdrant 를 일괄 구동한다.
- **프로덕션 계획**: 관리형 PostgreSQL, Redis · Qdrant 별도 VM, frontend-web 정적 호스팅.
