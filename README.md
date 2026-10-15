# 옷답노트 백엔드

Spring Boot 4.1 · Java 25 · PostgreSQL 17

## 필요한 것

- JDK 25
- Docker (Docker Desktop 또는 OrbStack)

## 처음 한 번

1. 환경변수 파일 만들기

   ```bash
   cp .env.example .env
   ```

2. `.env`의 빈 값(`DB_NAME`, `DB_USERNAME`, `DB_PASSWORD`)을 채운다.
   값은 팀 비밀 공유 경로에서 받는다. **`.env`는 커밋하지 않는다.**

## 실행

1. 로컬 DB 실행

   ```bash
   docker compose up -d
   ```

   `docker compose ps`에서 상태가 `healthy`가 될 때까지 기다린다.

2. 앱 실행: IntelliJ에서 `OtdapnoteApplication` 실행, 또는

   ```bash
   ./gradlew bootRun
   ```

3. 확인: http://localhost:8080/actuator/health 가 `{"status":"UP"}`

## 빌드·테스트

```bash
./gradlew build
```

테스트도 로컬 DB를 사용하므로 `docker compose up -d` 상태여야 한다.

## 종료·초기화

| 명령 | 동작 |
| --- | --- |
| `docker compose stop` | DB 컨테이너 멈춤 (데이터 유지) |
| `docker compose down` | 컨테이너 삭제 (데이터 유지) |
| `docker compose down -v` | 컨테이너와 **데이터까지 삭제** (DB를 처음 상태로) |

## 문제 해결

- **5432 포트가 이미 사용 중**: `.env`의 `DB_PORT`를 다른 값(예: 5433)으로 바꾸고 `docker compose up -d`를 다시 실행한다.
- **`Could not resolve placeholder 'DB_PASSWORD'`**: `.env`가 없거나 값이 비어 있다. IntelliJ 실행 설정의 Working directory가 저장소 루트인지도 확인한다.
- **DB 접속 정보를 바꿨는데 반영이 안 됨**: PostgreSQL 이미지는 볼륨이 비어 있을 때만 DB·계정을 만든다. `docker compose down -v` 후 다시 실행한다.