# SPRING PLUS

## 구현 목록

### Level 1

| # | 항목 | 주요 변경 내용 |
|---|------|--------------|
| 1 | @Transactional 이해 | `saveTodo()`에 `@Transactional` 추가 — 쓰기 트랜잭션 오버라이드 |
| 2 | JWT 이해 | `User`, `AuthUser`, `SignupRequest`, `JwtUtil`, `AuthService`, `JwtFilter` 전체에 `nickname` 추가 및 JWT claim 발급 |
| 3 | JPA 이해 | `searchTodos()` JPQL 추가 — `IS NULL OR` 패턴으로 weather/수정일 기간 선택적 검색 |
| 4 | 컨트롤러 테스트 | 기대 상태 코드 200 → 400, `AuthUser` 생성자 nickname 파라미터 추가 |
| 5 | AOP 이해 | `@After` → `@Before`, Pointcut 대상 `UserController.getUser` → `UserAdminController.changeUserRole` |

### Level 2

| # | 항목 | 주요 변경 내용 |
|---|------|--------------|
| 6 | JPA Cascade | `managers`에 `CascadeType.PERSIST` 추가, `Todo` 생성자에서 Manager 자동 등록 |
| 7 | N+1 | `CommentRepository` `JOIN` → `JOIN FETCH` 적용 |
| 8 | QueryDSL | `TodoRepositoryCustom` 인터페이스 + `TodoRepositoryImpl` 생성, `fetchJoin()`으로 N+1 방지, 기존 JPQL 삭제 |
| 9 | Spring Security | `SecurityConfig` 신규 생성, `JwtFilter`를 `OncePerRequestFilter`로 전환, `AuthUserArgumentResolver` SecurityContext 방식으로 수정 |

---

## 주요 변경 파일

| 파일 | 변경 내용 |
|------|-----------|
| `TodoService` | `saveTodo()`에 `@Transactional` 추가 |
| `User` | `nickname` 필드 추가, 생성자/`fromAuthUser` 수정 |
| `AuthUser` | `nickname` 필드 추가 |
| `SignupRequest` | `nickname` 필드 추가 |
| `JwtUtil` | `createToken()`에 `nickname` 파라미터 + claim 추가 |
| `AuthService` | 회원가입/로그인 시 `nickname` 전달 |
| `JwtFilter` | Servlet `Filter` → `OncePerRequestFilter`, claims에서 nickname 파싱, SecurityContext에 인증 정보 저장 |
| `AuthUserArgumentResolver` | `SecurityContextHolder`에서 `AuthUser` 꺼내도록 수정 |
| `SecurityConfig` | 신규 생성 — URL별 접근 권한 설정 |
| `QueryDslConfig` | 신규 생성 — `JPAQueryFactory` 빈 등록 |
| `TodoRepositoryCustom` | 신규 생성 — `findByIdWithUser` 인터페이스 |
| `TodoRepositoryImpl` | 신규 생성 — QueryDSL로 `findByIdWithUser` 구현 |
| `TodoRepository` | `TodoRepositoryCustom` extends 추가, 기존 JPQL 쿼리 삭제, `searchTodos()` 추가 |
| `TodoController` | `weather`, `startDate`, `endDate` 선택적 파라미터 추가 |
| `TodoService` (getTodos) | 검색 조건 파라미터 추가, `searchTodos()` 호출 |
| `CommentRepository` | `JOIN` → `JOIN FETCH` 적용 |
| `AdminAccessLoggingAspect` | `@After` → `@Before`, Pointcut 대상 수정 |
| `TodoControllerTest` | 기대 상태 코드 400으로 수정, `AuthUser` 생성자 nickname 추가 |
| `Todo` | `managers`에 `CascadeType.PERSIST` 추가, 생성자에서 Manager 자동 등록 |
