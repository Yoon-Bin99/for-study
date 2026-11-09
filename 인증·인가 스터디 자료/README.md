# 인증·인가 스터디 자료 (Spring Security)

인증·인가의 기본 개념부터 **Spring Security로 실제 로그인·권한 기능을 구현**하는 데 필요한 내용을 정리한 자료입니다.

## 기준 버전

| Spring Boot | Spring Security | 비고 |
|---|---|---|
| 3.x | 6.x | 현재 가장 많이 쓰는 버전 |
| 4.x | 7.x | 최신 (람다 DSL 필수, MFA 지원) |

- 예제 코드는 **두 버전 모두에서 동작하는 람다 DSL**로 작성했고, 버전별 차이가 있는 곳은 본문에 표시했습니다.
- 인터넷의 오래된 예제(`WebSecurityConfigurerAdapter`, `antMatchers`, `.and()`)는 현재 버전에서 동작하지 않습니다.
- 라이브러리 버전(jjwt 등)은 작성 시점(2026년 11월) 기준 예시입니다.

## 문서 구성

학습 목표 → 개념·동작 원리(그림) → 비교표 → 실습 코드 → 면접 질문 → 핵심 정리

## 목차

### 01.인증인가기초
> 프레임워크와 무관한 개념: 인증·인가, 비밀번호 해시, 세션, JWT, OAuth2·OIDC, 암호화 기초

- [01. 인증과 인가 (Authentication & Authorization)](./01.인증인가기초/01.인증과인가.md)
- [02. 비밀번호 안전하게 저장하기](./01.인증인가기초/02.비밀번호저장.md)
- [03. 세션과 쿠키 기반 인증](./01.인증인가기초/03.세션과쿠키.md)
- [04. 토큰과 JWT](./01.인증인가기초/04.토큰과JWT.md)
- [05. OAuth 2.0과 OpenID Connect](./01.인증인가기초/05.OAuth2와OIDC.md)
- [06. 암호화 기초: 인코딩·해시·대칭키·비대칭키](./01.인증인가기초/06.암호화기초.md)

### 02.스프링시큐리티구조
> Spring Security가 내부에서 어떻게 동작하는가: 필터 체인, 인증·인가 아키텍처, 예외 처리

- [01. Spring Security 개요와 필터 체인](./02.스프링시큐리티구조/01.스프링시큐리티개요와필터체인.md)
- [02. 인증 아키텍처 (Authentication)](./02.스프링시큐리티구조/02.인증아키텍처.md)
- [03. 인가 아키텍처 (Authorization)](./02.스프링시큐리티구조/03.인가아키텍처.md)
- [04. 인증·인가 예외 처리](./02.스프링시큐리티구조/04.예외처리.md)

### 03.스프링시큐리티실전
> 직접 구현하기: 세션 로그인, JWT, 리프레시 토큰, 소셜 로그인, 리소스 서버, 권한 설계, CORS·CSRF, 테스트

- [01. 폼 로그인과 세션 인증](./03.스프링시큐리티실전/01.폼로그인과세션인증.md)
- [02. JWT 인증 구현 (커스텀 필터 방식)](./03.스프링시큐리티실전/02.JWT인증구현.md)
- [03. 리프레시 토큰, 재발급, 로그아웃](./03.스프링시큐리티실전/03.리프레시토큰과로그아웃.md)
- [04. OAuth2 소셜 로그인 (카카오·구글)](./03.스프링시큐리티실전/04.OAuth2소셜로그인.md)
- [05. OAuth2 Resource Server로 JWT 검증하기 (Spring 공식 방식)](./03.스프링시큐리티실전/05.리소스서버와JWT검증.md)
- [06. 권한 설계와 메서드 보안](./03.스프링시큐리티실전/06.권한설계와메서드보안.md)
- [07. CORS와 CSRF 설정](./03.스프링시큐리티실전/07.CORS와CSRF설정.md)
- [08. Spring Security 테스트](./03.스프링시큐리티실전/08.시큐리티테스트.md)

### 04.보안심화
> 공격과 방어, MSA 인증, MFA·패스키 등 최신 기능

- [01. 인증·인가 관련 웹 보안 공격과 방어](./04.보안심화/01.웹보안공격과방어.md)
- [02. MSA 환경의 인증·인가](./04.보안심화/02.MSA환경의인증.md)
- [03. MFA, 패스키, 일회용 토큰 로그인 (Spring Security 최신 기능)](./04.보안심화/03.MFA와최신기능.md)

## 요청 하나가 처리되는 흐름

```
Authorization: Bearer <JWT>  또는  Cookie: JSESSIONID=...
   ↓
DelegatingFilterProxy → FilterChainProxy → SecurityFilterChain
   ├ SecurityContextHolderFilter   (세션에서 인증 정보 복원)
   ├ CorsFilter / CsrfFilter
   ├ 인증 필터                       (폼 로그인 · JWT 필터 · BearerToken · OAuth2 로그인)
   │    → AuthenticationManager → AuthenticationProvider → UserDetailsService + PasswordEncoder
   │    → SecurityContextHolder에 Authentication 저장
   ├ ExceptionTranslationFilter    (인증 실패 401 / 권한 없음 403)
   └ AuthorizationFilter           (URL 인가: hasRole, authenticated)
   ↓
DispatcherServlet → Controller(@AuthenticationPrincipal) → Service(@PreAuthorize, 소유자 검사)
```

## 추천 학습 순서

| 단계 | 문서 | 실습 |
|---|---|---|
| 1 | 01.인증인가기초 전체 | 세션과 JWT의 차이를 말로 설명해 보기 |
| 2 | 02.스프링시큐리티구조 01~02 | `logging.level.org.springframework.security=TRACE`로 필터 흐름 관찰 |
| 3 | 03.스프링시큐리티실전 01 | 폼 로그인 + DB 회원(UserDetailsService) |
| 4 | 02.스프링시큐리티구조 03~04 → 03.실전 02~03 | JWT 로그인 + 재발급 + 로그아웃 (Redis) |
| 5 | 03.실전 04 | 카카오·구글 로그인 붙이기 |
| 6 | 03.실전 06~08 | 소유자 검사, CORS·CSRF 정리, 401/403 테스트 표 작성 |
| 7 | 03.실전 05, 04.보안심화 | Resource Server 방식으로 리팩터링, MFA·패스키 실험 |

## 활용 팁

- 각 문서 끝 **면접 질문**은 답을 가리고 먼저 말해 본 뒤 확인하세요.
- `(→ 폴더/파일.md)`는 연결되는 문서입니다. HTTP·쿠키·TLS 기초는 「CS 스터디 자료」의 네트워크 문서와 이어집니다.
- 예제의 도메인(`example.com`), 키, 클라이언트 ID는 모두 가상의 값입니다. 실제 키와 비밀번호는 코드·Git에 넣지 말고 환경 변수나 Secrets Manager로 관리하세요.

총 4개 대분류, 21개 문서
