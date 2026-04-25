---
layout: post
title: Filter
subtitle: Filter
categories: TIL
tags: [TIL]
---

## Filter
- Servlet Filter는 서블릿 스펙의 일부로, 웹 애플리케이션의 요정(Request) 또는 응답(Response)을 가로채고 가공할 수 있는 재사용 가능한 컴포넌트이다.
- Spring에서도 이를 활용해 인증/인가 필터, 로깅 필터, CORS 필터 등을 만들 수 있다.

### Filter 동작 순서
1. 클라이언트의 요청이 먼저 필터를 통과함
2. 각 필터는 doFilter() 메서드를 통해 요청을 가로채고 필요 시 후속 필터 또는 서블릿으로 넘김
3. 서블릿/컨트롤러가 응답을 생성한 후 필터로 다시 돌아옴
4. 필터가 응답을 후처리하고 클라이언트로 전달

```
[HTTP 요청]
   ↓
[FilterChain: 여러 필터]
   ↓
[DispatcherServlet 또는 서블릿]
   ↓
[Controller 등 비즈니스 로직]
   ↓
[응답 생성]
   ↑
[Filter: 응답 후처리]
   ↑
[클라이언트]

```

### Filter 사용 예시
- 로그인 검증
- JWT 토큰 검사
- CORS 처리
- 요청/응답 로깅
- 응답 캐싱

### Filter 구현 예제

```java
@WebFilter(urlPatterns = "/*")
public class LoggingFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
                         FilterChain chain) throws IOException, ServletException {
        HttpServletRequest req = (HttpServletRequest) request;
        System.out.println("Request URI: " + req.getRequestURI());

        chain.doFilter(request, response); // 다음 필터 또는 서블릿으로 전달

        System.out.println("Response processed");
    }
}
```

- Spring에서 필터 등록 ( Spring boot )

```java
@Configuration
public class FilterConfig {
    @Bean
    public FilterRegistrationBean<LoggingFilter> loggingFilter() {
        FilterRegistrationBean<LoggingFilter> registration = new FilterRegistrationBean<>();
        registration.setFilter(new LoggingFilter());
        registration.addUrlPatterns("/*");
        return registration;
    }
}
```

### Filter 주요 사용 예

| 목적      | 설명                             |
| ------- | ------------------------------ |
| 인증 처리   | 로그인 여부 확인 후 인증되지 않은 사용자는 접근 제한 |
| CORS 처리 | 응답에 CORS 헤더를 추가                |
| 로깅      | 요청 URI, 처리 시간, 사용자 정보 등 기록     |
| 인코딩     | UTF-8 등의 문자 인코딩 설정             |
| 보안 검사   | 요청 헤더, 파라미터에 대한 악성코드 검사 등      |

### Filter의 장단점

- 장점
1. 요청/응답 흐름 제어 가능 (컨트롤러 밖에서 동작)
2. 관심사 분리 (예: 인증, 로깅 등을 독립적으로 관리)
3. 다양한 URL에 공통 처리 적용 가능

- 단점
1. 순서를 잘못 구성하거나 무분별하게 사용하면 디버깅이 어려움
2. 비즈니스 로직과 동떨어져 있어 테스트가 어려울 수 있음
3. Spring Security 필터와 충돌 우려가 있음 (주의 필요)

### 관련 개념

| 개념                    | 설명                                                                                |
| --------------------- | --------------------------------------------------------------------------------- |
| Interceptor           | Spring MVC 전용. Handler 실행 전/후 동작 가능. 컨트롤러 수준에서 동작함.                               |
| AOP                   | 메서드 단위의 공통 관심사를 처리. 트랜잭션, 로깅 등에서 사용됨.                                             |
| Filter vs Interceptor | Filter는 서블릿 단위(DispatcherServlet 이전), Interceptor는 컨트롤러 단위(DispatcherServlet 이후). |

### Spring 환경에서의 필터 순서

- Spring Boot에서는 다음과 같은 순서로 필터가 실행된다:
1. Servlet Filter (javax.servlet.Filter)
2. Spring Security Filter (내부에서 여러 FilterChain 구성됨)
3. DispatcherServlet
4. Interceptor
5. Controller

- 필터 순서 조정이 필요하다면 `FilterRegistrationBean#setOrder()`를 통해 설정 가능하다.

