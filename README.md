# Meerkatgram

사용자가 이미지 게시글을 올리고 소통하는 **커뮤니티형 웹 애플리케이션**입니다.  
백엔드(Spring Boot 3)와 프론트엔드(Vue 3)는 분리된 프로젝트로 관리하며 HTTP API로 통신합니다.

---

## 화면 디자인

![Design](https://github.com/user-attachments/assets/36813f6a-a155-4ffe-a341-8f4413fa4520)

---

## ERD

![ERD](https://github.com/user-attachments/assets/4ae34858-7936-415a-9081-d7b3435343d3)

---

## 기술 스택

| 영역 | 기술 |
|------|------|
| Language | Java 17 |
| Backend | Spring Boot 3.5, Spring Security, MyBatis 3.0.5 |
| Database | MySQL 8.4 |
| Auth | JWT (Access / Refresh Token) |
| Frontend | Vue 3 (Composition API), Pinia, Vue Router 4, Axios, Vite |
| Build | Gradle |

---

## 주요 기능

| 분류 | 기능 |
|------|------|
| 인증 | 회원가입, 로그인, 로그아웃, Access Token 재발급 |
| 유저 | 유저 정보 조회 |
| 게시글 | 목록(페이지네이션), 상세 조회, 작성, 삭제 |
| 파일 | 게시글 이미지 업로드, 프로필 이미지 업로드 |

---

## 상세 문서

| 문서 | 설명 |
|------|------|
| [프로젝트 개요](./meerkatgram-doc/1st-doc/01-project-overview.md) | 기능 목록, 기술 스택, 요청 처리 흐름 |
| [ERD & 데이터베이스](./meerkatgram-doc/1st-doc/02-erd-and-database.md) | 테이블 스키마, 관계, 소프트 삭제 패턴 |
| [백엔드 아키텍처](./meerkatgram-doc/1st-doc/03-backend-architecture.md) | 레이어 구조, 패키지 설계 |
| [API 명세](./meerkatgram-doc/1st-doc/04-api-specification.md) | 전체 엔드포인트, 요청/응답 예시 |
| [JWT 인증 가이드](./meerkatgram-doc/1st-doc/05-auth-jwt-guide.md) | 토큰 발급·갱신·무효화 흐름 |
| [주요 기능 가이드](./meerkatgram-doc/1st-doc/06-key-features-guide.md) | 게시글, 파일 업로드 등 구현 상세 |
| [개발 환경 세팅](./meerkatgram-doc/1st-doc/07-setup-guide.md) | 로컬 실행 방법 |

---

## 실행 방법

### 요구 환경

- Java 17
- MySQL 8.4
- 함께 사용할 Vue 클라이언트: [msa4-meerkatgram-client-main](https://github.com/jeon950523/msa4-meerkatgram-client-main)

### 1. 데이터베이스 준비

```sql
CREATE DATABASE meerkatgram
  DEFAULT CHARACTER SET utf8mb4
  COLLATE utf8mb4_0900_ai_ci;
```

`src/main/resources/schema/v000`의 SQL을 번호 순서대로 적용하고, 필요하면 `src/main/resources/dummy`의 샘플 데이터를 추가합니다.

### 2. 애플리케이션 설정

`src/main/resources/application.yaml`을 만들고 DB 접속 정보, JWT Secret, 파일 저장 경로와 CORS 허용 주소를 설정합니다. 전체 예시는 [개발 환경 세팅 문서](./meerkatgram-doc/1st-doc/07-setup-guide.md)를 참고합니다.

### 3. 빌드와 실행

```powershell
.\gradlew.bat build
.\gradlew.bat bootRun
```

서버는 기본적으로 `http://localhost:8080`에서 실행됩니다.

## 사용 방법

1. 프로필 이미지를 업로드하고 반환된 URL로 회원가입합니다.
2. 로그인 응답의 Access Token을 API 요청의 `Authorization: Bearer ...` 헤더에 넣습니다.
3. 게시글 이미지를 업로드한 뒤 이미지 URL과 내용을 사용해 게시글을 작성합니다.
4. `/api/posts`에서 페이지별 게시글을 조회하고 상세·내 게시글 화면을 확인합니다.
5. 본인이 작성한 게시글만 삭제할 수 있습니다.
6. Access Token이 만료되면 Refresh Token 쿠키로 `/api/reissue-token`을 호출합니다.
7. 로그아웃하면 서버의 Refresh Token과 브라우저 쿠키가 함께 무효화됩니다.

## 커밋 이력

| 순서 | 커밋 | 이전 단계에서 변경한 내용 |
| ---: | --- | --- |
| 1 | `63f6944` 초기 생성 | Spring Boot 프로젝트와 기본 사용자·게시글 구조를 생성했습니다. |
| 2 | `e06de17` DB 스키마 | users·posts 테이블과 FK 스키마를 추가했습니다. |
| 3 | `21f6961` 환경설정 제외 | 비밀번호와 Secret이 포함될 수 있는 설정 파일을 Git 추적 대상에서 제외했습니다. |
| 4 | `a634d54` 설정 예제 | 제외된 실제 설정 대신 실행에 필요한 예제 설정을 추가했습니다. |
| 5 | `a4e1e4f` 게시글 더미 데이터 | 페이지네이션과 조회 기능을 확인할 샘플 게시글을 추가했습니다. |
| 6 | `e852ebf` 예외 처리 | Controller별 오류 대신 전역 예외 처리 기반을 추가했습니다. |
| 7 | `026bac4` Security 임시 해제 | 기능 개발 중 API 호출을 확인할 수 있도록 일부 보안 설정을 임시 조정했습니다. |
| 8 | `3b98cd8` 공통 응답 | 모든 API가 code·message·data 구조를 사용하도록 GlobalRes를 추가했습니다. |
| 9 | `8d52522` 페이지네이션 | MyBatis LIMIT/OFFSET, 전체 개수와 마지막 페이지 계산을 구현했습니다. |
| 10 | `93d9166` 요청 오타 수정 | 페이지네이션 요청 DTO의 필드 오타를 정리했습니다. |
| 11 | `41945e9` JWT 로그인 시작 | Spring Security, JWT Provider, 인증 DTO와 관련 설정을 대규모로 추가했습니다. |
| 12 | `361bec9` JWT 보완 | JWT 생성·검증과 인증 흐름의 누락 부분을 이어서 구현했습니다. |
| 13 | `abd64a8` 로그인 구조 정리 | 로그인 관련 Service·Mapper·응답 구조를 수정하고 불필요한 부분을 정리했습니다. |
| 14 | `2071647` 로그인·재발급 완료 | Access Token 발급과 Refresh Token 쿠키·DB 비교 기반 재발급을 완성했습니다. |
| 15 | `d320295` 로그아웃·필터 | 로그아웃 시 토큰 무효화와 요청별 JWT 검증 필터를 완성했습니다. |
| 16 | `1673b87` 회원가입·이미지 | BCrypt 회원가입, 프로필·게시글 이미지 업로드와 저장 경로 제공을 추가했습니다. |
| 17 | `3f8de86` DB 예외·사용자 응답 | DB 예외 응답을 추가하고 사용자 응답에 ID와 작성 게시글 수를 포함했습니다. |

### 첫 번째 커밋에서 두 번째 커밋으로

첫 커밋은 애플리케이션 골격과 Java 계층을 만든 단계이고, 두 번째 커밋은 이를 실제 MySQL 데이터 모델과 연결한 단계입니다. 즉 메모리상의 코드 구조에서 users·posts 관계를 가진 영속화 구조로 넘어갔습니다.

## 성능 개선 기록

페이지네이션 도입은 전체 게시글을 한 번에 전달하지 않도록 데이터 전송량을 제한하는 구조적 개선입니다. 다만 커밋에는 전체 조회와 페이지 조회의 응답 시간·메모리 사용량 비교가 없어 개선율은 수치로 기록하지 않았습니다.

향후 동일 데이터 건수에서 전체 조회와 LIMIT/OFFSET 조회의 p50/p95 응답 시간, 전송 바이트와 DB 실행 계획을 비교해야 합니다.
