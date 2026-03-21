# 진행 현황 (Progress)

> 마지막 업데이트: 2026-03-16
> 다음에 Claude Code를 열면 이 파일부터 읽고 이어서 진행하세요.

---

## 프로젝트 개요

**목표**: Spring Boot(백엔드) + React(프론트엔드)로 게시판을 처음부터 직접 만들며 학습

**사용자 수준**
- Java: 기초 있음
- Spring Boot: 처음 배우는 중
- React / JavaScript: 완전 입문자

**학습 방식**: 개념 설명 → 직접 작성 → 확인/피드백 순서로 진행

---

## 환경 정보

| 항목 | 값 |
|------|-----|
| Spring Boot | 4.0.3 |
| Java | 21 |
| 빌드 도구 | Gradle Kotlin DSL (`build.gradle.kts`) |
| 패키지명 | `com.example.board` |
| 백엔드 경로 | `/Users/chosunghoon/Desktop/GitHub/Repository/Spring-Boot_practice/board-project` |
| MySQL 설치 | 공식 설치 프로그램 (Homebrew 아님) |
| MySQL 계정 | root (비밀번호 있음) |
| DB 이름 | `board_db` |
| 프론트엔드 | 아직 미생성 (Vite + React 예정) |

> **주의**: Spring Boot 4.x는 `javax.*` → `jakarta.*` 패키지를 사용함

---

## 완료된 항목

### ✅ Step 1-1: 기존 프로젝트 삭제
- `SpringBoot_practice` 폴더 삭제 완료

### ✅ Step 1-2: 백엔드 프로젝트 생성
- Spring Initializr로 생성
- 폴더명: `board-project`
- 의존성: Spring Web, Spring Data JPA, MySQL Driver, Lombok, Validation

### ✅ MySQL 환경 설정
- MySQL 로컬 설치 확인
- `board_db` 데이터베이스 생성 완료
- root 계정 접속 확인

---

## 현재 중단 지점

### ⏳ Step 1-3: application.yml 작성 ← **여기서부터 시작**

`application.properties`를 삭제하고 `application.yml`로 교체해야 함.

**파일 위치**: `board-project/src/main/resources/application.yml`

**작성할 내용**:
```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/board_db?useSSL=false&serverTimezone=Asia/Seoul&characterEncoding=UTF-8
    username: root
    password: [MySQL root 비밀번호 입력 필요]
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: create
    show-sql: true
    properties:
      hibernate:
        format_sql: true
        dialect: org.hibernate.dialect.MySQL8Dialect
```

**다음 Claude에게**: 사용자에게 MySQL root 비밀번호를 물어보고 application.yml을 작성해 주세요.
그 다음은 Step 1-4 (프론트엔드 프로젝트 생성)으로 이어갑니다.

---

## 이후 진행 순서

자세한 내용은 `curriculum.md` 참고.

1. Step 1-3: application.yml 작성 (MySQL 비밀번호 필요)
2. Step 1-4: Vite + React 프론트엔드 프로젝트 생성
3. Phase 2: 백엔드 데이터 계층 (Entity, Repository)
4. Phase 3~8: 순서대로 진행
