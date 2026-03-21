# 전체 커리큘럼 (Curriculum)

Spring Boot + React 게시판 프로젝트 전체 학습 계획.
각 단계의 완료 여부는 `progress.md`에서 확인.

---

## 전체 Phase 흐름

```
Phase 1: 프로젝트 초기 설정
Phase 2: 백엔드 - 데이터 계층 (Entity, Repository)
Phase 3: 백엔드 - 비즈니스 계층 (DTO, Service)
Phase 4: 백엔드 - API 계층 (Controller, 예외처리)
Phase 5: 프론트엔드 - React 기초 + 프로젝트 설정
Phase 6: 프론트엔드 - 게시글 목록/상세 구현
Phase 7: 프론트엔드 - 게시글 작성/수정/삭제
Phase 8: 프론트엔드 - 댓글 기능
```

---

## Phase 1: 프로젝트 초기 설정

| 상태 | Step | 내용 |
|------|------|------|
| ✅ | Step 1-1 | 기존 프로젝트 삭제 |
| ✅ | Step 1-2 | Spring Initializr로 백엔드 프로젝트 생성 |
| ⏳ | Step 1-3 | `application.yml` 작성 (MySQL 연결, JPA 설정) |
| 🔲 | Step 1-4 | Vite + React 프론트엔드 프로젝트 생성 |

### Step 1-3 상세
- `application.properties` 삭제 후 `application.yml` 작성
- 설정 항목: datasource (url/username/password/driver), jpa (ddl-auto/show-sql/format-sql/dialect)
- `ddl-auto: create` → 앱 실행 시 테이블 자동 생성

### Step 1-4 상세
```bash
npm create vite@latest board-frontend -- --template react
cd board-frontend
npm install
npm install axios react-router-dom
```

---

## Phase 2: 백엔드 - 데이터 계층

| 상태 | Step | 내용 |
|------|------|------|
| 🔲 | Step 2-1 | `BaseEntity.java` |
| 🔲 | Step 2-2 | `Post.java` (Entity) |
| 🔲 | Step 2-3 | `Comment.java` (Entity) |
| 🔲 | Step 2-4 | `PostRepository.java` / `CommentRepository.java` |

### Step 2-1: BaseEntity
- 개념: JPA Auditing, `@MappedSuperclass`
- 필드: `createdAt`, `updatedAt` (자동 관리)
- 메인 클래스에 `@EnableJpaAuditing` 추가 필요

### Step 2-2: Post Entity
- 개념: `@Entity`, `@Table`, `@Column`, `@Id`, `@GeneratedValue`
- 필드: id, title, content, author, viewCount
- 연관관계: `@OneToMany` (Comment와)

### Step 2-3: Comment Entity
- 개념: `@ManyToOne`, 외래키(FK)
- 필드: id, content, author, post(FK)

### Step 2-4: Repository
- 개념: `JpaRepository`, Spring Data JPA 쿼리 메서드
- `findAllByOrderByCreatedAtDesc()` (최신순 목록)
- `findByTitleContaining()` (제목 검색)

---

## Phase 3: 백엔드 - 비즈니스 계층

| 상태 | Step | 내용 |
|------|------|------|
| 🔲 | Step 3-1 | Request DTO (`PostRequestDto`, `CommentRequestDto`) |
| 🔲 | Step 3-2 | Response DTO (`PostResponseDto`, `PostListResponseDto`, `CommentResponseDto`) |
| 🔲 | Step 3-3 | `PostService` / `PostServiceImpl` |
| 🔲 | Step 3-4 | `CommentService` / `CommentServiceImpl` |

### Step 3-1: Request DTO
- 개념: DTO란 무엇인가, 왜 Entity를 직접 쓰지 않는가
- `@NotBlank`, `@Size` 유효성 검사 애노테이션 사용

### Step 3-2: Response DTO
- `PostResponseDto`: 상세 조회용 (댓글 목록 포함)
- `PostListResponseDto`: 목록 조회용 (요약 정보만)
- 정적 팩토리 메서드 `from()` 패턴 사용

### Step 3-3: PostService
- 개념: 인터페이스와 구현체 분리 이유
- `@Transactional`, `readOnly = true` 최적화
- 더티 체킹: viewCount 증가 시 `save()` 없이 자동 UPDATE

---

## Phase 4: 백엔드 - API 계층

| 상태 | Step | 내용 |
|------|------|------|
| 🔲 | Step 4-1 | `PostController.java` |
| 🔲 | Step 4-2 | `CommentController.java` |
| 🔲 | Step 4-3 | 예외 처리 (GlobalExceptionHandler) |
| 🔲 | Step 4-4 | 백엔드 API 테스트 |

### Step 4-1: PostController
- 개념: REST API, HTTP 메서드 (GET/POST/PUT/DELETE)
- `@RestController`, `@RequestMapping`, `@PathVariable`, `@RequestBody`
- `ResponseEntity`로 HTTP 상태 코드 반환

### Step 4-2: CommentController
- 중첩 URL 구조: `/api/posts/{postId}/comments`

### Step 4-3: 예외 처리
- `PostNotFoundException.java`
- `GlobalExceptionHandler.java` (`@RestControllerAdvice`)
- `ErrorResponse.java`

### Step 4-4: API 테스트
- Swagger UI 또는 curl/Postman으로 각 엔드포인트 동작 확인

---

## Phase 5: 프론트엔드 - React 기초

| 상태 | Step | 내용 |
|------|------|------|
| 🔲 | Step 5-1 | React 핵심 개념 학습 |
| 🔲 | Step 5-2 | 프로젝트 폴더 구조 설정 |
| 🔲 | Step 5-3 | axios 설정 + CORS 처리 |

### Step 5-2: 폴더 구조
```
src/
├── api/          ← axios API 호출 모음
├── pages/        ← 각 페이지 컴포넌트
├── components/   ← 재사용 컴포넌트
└── App.jsx       ← 라우터 설정
```

### Step 5-3: axios + CORS
- `api/postApi.js`, `api/commentApi.js` 작성
- 백엔드에서 `@CrossOrigin` 또는 WebMvcConfigurer로 CORS 허용

---

## Phase 6: 프론트엔드 - 목록/상세

| 상태 | Step | 내용 |
|------|------|------|
| 🔲 | Step 6-1 | `PostListPage.jsx` (게시글 목록) |
| 🔲 | Step 6-2 | `PostDetailPage.jsx` (게시글 상세 + 댓글) |

---

## Phase 7: 프론트엔드 - 작성/수정/삭제

| 상태 | Step | 내용 |
|------|------|------|
| 🔲 | Step 7-1 | `PostCreatePage.jsx` (게시글 작성) |
| 🔲 | Step 7-2 | `PostEditPage.jsx` (게시글 수정) |
| 🔲 | Step 7-3 | 삭제 버튼 구현 |

---

## Phase 8: 프론트엔드 - 댓글

| 상태 | Step | 내용 |
|------|------|------|
| 🔲 | Step 8-1 | `CommentForm.jsx` (댓글 작성 폼) |
| 🔲 | Step 8-2 | `CommentList.jsx` (댓글 목록, 수정, 삭제) |

---

## 학습 원칙

1. **매 단계마다**: 개념 설명 → 직접 작성 → 확인/피드백
2. **백엔드 설명**: Java 기초는 알고 있으므로 JPA/Spring 개념 위주로
3. **프론트엔드 설명**: JavaScript 문법부터 함께 설명 (완전 입문자)
4. **검증**: 각 Phase 완료 후 실제 실행하여 동작 확인

---

## 상태 범례

| 아이콘 | 의미 |
|--------|------|
| ✅ | 완료 |
| ⏳ | 진행 중 / 중단 지점 |
| 🔲 | 미진행 |
