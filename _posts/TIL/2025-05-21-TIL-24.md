---
layout: post
title: Cookie, Session, Token
subtitle: 인증 / 인가
categories: TIL
tags: [TIL]
---

## Cookie
- 사용자의 웹 브라우저에 저장되는 정보로 사용자의 상태 혹은 세션을 유지하거나 사용자 경험을 개선하기 위해 사용된다.
- 서버가 `Set-Cookie` 해더를 통해 브라우저에게 쿠키를 저장하게 하고, 이후 브라우저는 요청마다 쿠키를 `Cookie`헤더에 실어 보낸다.
- 간단하게 `클라이언트에 저장되는 작은 데이터 파일`이다.

### Cookie 특징
- 클라이언트에 저장되며, 보통 키 - 값 쌍으로 구성된다.
- 로그인 정보, 세션 ID 등 상태 유지를 위한 정보를 담는데 사용된다.
- 기본적으로는 보안에 취약하므로 `HttpOnly`,`Secure`,`SameSite`등의 옵션을 설정하여 보안을 강화한다.

### Cookie를 사용하는 이유
1. Http는 Stateless, Connectionless 특성을 가지고 있다.
2. Client가 재요청시 Server는 이전 요청에 대한 정보를 기억하지 못한다.
3. 로그인과 같이 상태를 유지해야 하는 경우가 발생한다.
4. Request에 사용자 정보를 포함하면 해결이 된다 -> 로그인 후에는 사용자 정보와 관련된 값이 저장되어 있어야 한다.
5. 브라우저를 완전히 종료한 뒤 다시 열어도 사용자 정보가 유지되어야 한다.
6. 서버에 전송하지 않고 브라우저에 단순히 데이터를 저장하고 싶다면 Web Storage를 사용하면 되지만 보안에 취약하기 때문에 주민번호와 같은 민감정보를 저장하면 안된다!

### Cookie 동작 방법

1. 로그인 성공시 응답
- Set-Cookie
- 로그인시 전달된 ID,PassWord로 User 테이블 조회하여 일치 여부 확인
- 일치한다면 Set-Cookie를 활용해 Cookie에 사용할 값 저장 ( Cookie는 보안에 취약하다. )

2. 로그인 이후 요청
- 요청 헤더 `Cookie : 사용자 정보`
- 로그인 이후에는 모든 요청마다 Request Header에 항상 Cookie 값을 담아서 요청한다
- 네트워크 트래픽이 추가적으로 발생한다
- 최소한의 정보만 사용해야한다
- Cookie에 담겨있는 값으로 인증/인가를 진행한다.

### Cookie 보안

1. `Secure` 
- 기본적으로 Cookie는 http,https 구분하지 않고 전송한다.
- Secure를 적용하면 https인 경우에만 전송한다 ( s = Secure )

2. `HttpOnly`
- XSS(Cross-site Scription) 공격을 방지한다 : 악성 스크립트를 웹 페이지에 삽입하여 다른 사용자의 브라우저에서 실행되도록 하는 공격
- 자바스크립트에서 Cookie 접근을 못하도록 막는다.
- HTTP 요청시 사용한다

3. `SameSite` 
- 비교적 최신 기능이라 브라우저 지원여부를 확인 해야한다.
- CSRF(Cross-Site Request Forgery) 공격을 방지한다 : 사용자가 의도하지 않은 상태에서 특정 요청을 서버에 전송하게 하여 사용자 계정에서 원치 않는 행동을 하게 만든다.
- 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송

### Cookie의 문제점
1. 쿠키 값은 임의로 변경할 수 있다
2. Cookie에 저장된 Data는 탈취되기 쉽다.
3. 용량이 제한되어 있다.


## Session
- 세션은 하나의 사용자와 서버 간의 연결을 식별하고 상태를 유지하기 위한 서버 측 저장소이다.
- 사용자가 웹사이트에 접속하면 서버는 고유한 세션 ID를 발급하고, 그 ID를 기준으로 사용자 정보를 서버에 저장한다.
- 이 세션ID는 클라이언트 측에 쿠키(`JSESSIONID`)로 전달되어, 이후 요청마다 자동으로 서버에 전송된다.

### Session의 동작순서
1. 사용자가 서버에 최초 요청 → 서버가 세션 생성
2. 서버가 세션 ID (JSESSIONID)를 쿠키로 클라이언트에 전달
3. 클라이언트가 이후 요청을 보낼 때 세션 ID 쿠키를 함께 전송
4. 서버는 해당 세션 ID에 연결된 사용자 정보를 꺼내어 처리

### Session 저장 위치
- Spring Boot는 기본적으로 세션을 서버의 메모리에 저장하지만, 필요에 따라 다음으로 저장소를 확장할 수 있다.
- 기본 : In-Memory
- 외부 저장소 : Redis, JDBC, Hazelcast 등등.. -> 대규모/분산 시스템에서 세션 클러스터링을 위해 필요하다.

### Session 사용 방식
- 세션에 데이터 저장하기

```java
@GetMapping("/login")
public String login(HttpSession session) {
    session.setAttribute("userId", "abc123"); // 세션에 저장
    return "로그인 완료";
}
```

- 세션에서 데이터 꺼내기

```java
@GetMapping("/mypage")
public String myPage(HttpSession session) {
    String userId = (String) session.getAttribute("userId");
    if (userId == null) return "로그인이 필요합니다.";
    return "유저 ID: " + userId;
}
```

- 세션 무효화 ( 로그아웃 )

```java
@GetMapping("/logout")
public String logout(HttpSession session) {
    session.invalidate(); // 세션 만료
    return "로그아웃 완료";
}
```

### Session 보안 관련 주의사항

| 보안 이슈                       | 설명 및 대처                                                              |
| --------------------------- | -------------------------------------------------------------------- |
| 세션 하이재킹                     | 세션 ID 탈취 방지 → HTTPS, `Secure` 쿠키 사용                                  |
| 세션 고정 공격 (Session Fixation) | 로그인 시 세션 재생성: `session.invalidate()` 후 새로 생성                         |
| XSS                         | 세션 자체는 서버에 있지만, 세션 ID는 쿠키로 전달되므로 `HttpOnly` 설정 필요                    |
| CSRF                        | 세션 기반 인증은 CSRF 공격에 취약할 수 있음 → CSRF 토큰 사용 필수 (Spring Security가 기본 제공) |

## Token
- 사용자 인증 정보를 포함한 문자열 데이터로, 사용자가 로그인하면 서버가 토큰을 발급하고, 클라이언트는 이 토큰을 저장한 뒤 이후 요청마다 함께 보낸다.
- 서버는 이 토큰을 검증하여 사용자를 식별한다.
- 가장 많이 사용하는 포맷 : JWT ( JSON Web Token)

### Token의 구조 설명

| 부분            | 내용                                          |
| ------------- | ------------------------------------------- |
| **Header**    | 토큰 타입과 서명 알고리즘 (`alg`, `typ`)               |
| **Payload**   | 사용자 정보(Claims), 예: `userId`, `roles`, `exp` |
| **Signature** | 위조 방지용 서명. 서버의 비밀 키로 생성됨                    |

### Token의 동작 방식
1. 사용자가 로그인하면 서버가 사용자 정보를 기반으로 JWT 생성 후 반환
2. 클라이언트는 JWT를 로컬 스토리지, 세션 스토리지 또는 쿠키에 저장
3. 이후 요청시 JWT를 HTTP 헤더에 담아 전송
4. 서버는 토큰을 검증 -> 사용자 식별

### JWT를 이용한 인증 절차 ( Spring Security와 연동 )
- 기본 흐름 :
1. 로그인 API에서 사용자 인증 후 JWT 발급
2. 클라이언트가 토큰을 `Authorization` 헤더에 담아 API 호출
3. 서버는 JWT를 파싱해 사용자 정보 추출 -> 인증 객체 생성

- 필요 구성 요소 :
1. `JWTUtil` (토큰 생성/검증 유틸 클래스)
2. `JwtAuthenticationFilter` (JWT를 검사하고 SecurityContext에 인증 객체 저장)
3. `SecurityConfig` (Spring Security 설정)

### Token의 보안 이슈 및 대책

| 위험 요소          | 설명                             | 대책                       |
| -------------- | ------------------------------ | ------------------------ |
| **탈취**         | 누군가 JWT를 훔치면 누구나 인증된 것처럼 사용 가능 | HTTPS 사용, 토큰 만료 시간 짧게 설정 |
| **위조**         | 사용자가 JWT를 조작할 수 있음             | 서명(Signature)으로 위조 방지    |
| **만료된 토큰 재사용** | 만료된 토큰을 계속 사용하는 경우             | 만료 검증, 리프레시 토큰 사용        |

### 액세스 토큰 vs 리프레시 토큰

| 항목    | 액세스 토큰 (Access Token) | 리프레시 토큰 (Refresh Token) |
| ----- | --------------------- | ----------------------- |
| 용도    | 실제 API 호출에 사용         | 액세스 토큰 갱신에 사용           |
| 만료 시간 | 짧음 (보통 15분 \~ 1시간)    | 김 (보통 며칠 \~ 몇 주)        |
| 저장 위치 | 보통 클라이언트 (메모리/쿠키)     | 보통 쿠키 또는 DB             |

## Cookie vs Session vs Token 요약

| 항목       | 토큰 (JWT)       | 세션             | 쿠키             |
| -------- | -------------- | -------------- | -------------- |
| 저장 위치    | 클라이언트          | 서버             | 클라이언트          |
| 인증 상태 저장 | 없음 (Stateless) | 서버가 상태 저장      | 없음 (데이터만 저장)   |
| 확장성      | 좋음 (분산 환경에 유리) | 낮음 (서버 메모리 필요) | 보통             |
| 보안       | 위조 방지 서명 필요    | 세션 ID 탈취 위험 있음 | XSS/CSRF 방지 필요 |
