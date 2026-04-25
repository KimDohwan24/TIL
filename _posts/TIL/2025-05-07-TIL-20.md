---
layout: post
title: Spring 데이터 흐름
subtitle: MVC, 3-Layer
categories: TIL
tags: [TIL]
---

## Spring 
- 자바 기반의 웹 애플리 케이션 개발을 위한 프레임 워크(오픈소스) 이다.   

| 구분   | 내용 |
|--------|------|
|  장점 | - 경량 프레임워크로 유연한 모듈화 가능 |
|        | - 의존성 주입(DI)으로 낮은 결합도 유지 |
|        | - AOP로 공통 기능 분리 및 재사용성 향상 |
|        | - Spring Boot 등 풍부한 생태계 지원 |
|        | - 자동 설정, 내장 서버로 빠른 개발 가능 |
|        | - 활발한 커뮤니티와 풍부한 공식 문서 |
|        | - 테스트 용이 (단위/통합 테스트) |
|        | - 보안(Spring Security) 및 트랜잭션 처리 지원 |
|  단점 | - 진입 장벽 높고 학습 곡선이 가파름 |
|        | - 복잡한 설정 및 설정 파일 증가 가능성 |
|        | - 추상화 남용 시 유지보수/디버깅 어려움 |
|        | - 빈 관리로 인한 메모리/성능 오버헤드 발생 |
|        | - 라이브러리 간 버전 호환성 문제 가능성 |
|        | - 자동 설정이 불투명하여 동작 파악 어려움 |

### Spring MVC
- 웹 요청을 처리하기 위한 구조화된 방식을 제공하며, 핵심은 요청 분리와 책임 분담이다.
- Model : 비즈니스 로직, 데이터 객체 ( DTO, Entity 등.. )
- View : 사용자에게 보여지는 화면 ( HTML, Thymeleaf, JSP, 등.. )
- Controller : 사용자 요청을 받아 처리하고, 결과를 View에 전달

#### Spring MVC 흐름 예시
1. 사용자가 ``/users`` URL 요청
2. DIspatcherServlet이 요청을 가로채서 Controller에 전달
3. Controller는 서비스 호출 후 결과를 Model에 담음
4. ViewResolver가 적절한 View를 찾아 응답 렌더링


### 3-Layer 아키텍처
- 애플리케이션을 역할별로 세 계층으로 나누어 구성하는 설계 패턴이다.   

| 계층                   | 주요 클래스                      | 역할                      |
| -------------------- | -------------------------------- | ----------------------- |
| **Controller Layer** | `@Controller`, `@RestController` | 요청 수신, 사용자 입력 처리, 결과 반환 |
| **Service Layer**    | `@Service`                       | 비즈니스 로직 처리, 트랜잭션 처리     |
| **Repository Layer** | `@Repository`, JPA, MyBatis 등    | DB 접근, CRUD 처리          |


#### Servlet
- 자바 기반의 웹 서버에서 HTTP 요청과 응답을 처리하는 자바 클래스이다.
- 특징 : 동적으로 웹 콘텐츠 생성 ( 예 : JSON, HTML ) , Java EE 표준 API, HTTP 프로토콜을 기반으로 작동 ( `HttpServlet` 상속 )
- 기본 동작 흐름 : [클라이언트] → [서버: URL 요청] → [Servlet] → [응답 전송] 

#### Servlet Container
- Servlet을 실행하고 관리하는 웹 서버/환경이다.
- Servlet클래스의 `생명주기(Lifecycle)`를 관리하며, HTTP 요청을 Servlet으로 연결해주는 역할이다.
- 예 : `Tomcat` , Jetty, Undertow, WildFly, GlassFish ...

#### Servlet Container 주요 역할

| 역할         | 설명                                                      |
| ---------- | ------------------------------------------------------- |
| 요청 라우팅     | 클라이언트 요청을 적절한 Servlet으로 전달                              |
| Servlet 관리 | 객체 생성, 초기화(`init()`), 서비스(`service()`), 종료(`destroy()`) |
| 보안 처리      | 인증/인가, 세션 관리 등 수행                                       |
| 스레드 관리     | 다중 사용자 요청을 위한 스레드 처리                                    |

#### Spring MVC와의 관계
- Spring MVC는 내부적으로 Servlet을 기반으로 동작한다.
- `DispatcherServlet`이라는 Spring에서 제공하는 핵심 Servlet이 모든 HTTP 요청을 받아 처리한다.
- 흐름 예시
1. 사용자 요청 → 톰캣(Servlet Container)
2. 톰캣이 DispatcherServlet 호출
3. DispatcherServlet → Controller 호출
4. Controller → Service → Repository
5. 응답 반환 → DispatcherServlet → 사용자


### 요약

| 개념                | 설명                                                                                      |
|---------------------|---------------------------------------------------------------------------------------------|
| **Servlet**         | HTTP 요청과 응답을 처리하는 자바 클래스 (`HttpServlet` 등)                                  |
| **Servlet Container** | Servlet의 생명주기 관리 및 요청을 Servlet에 전달하는 실행 환경 (예: Tomcat)                   |
| **Spring MVC**      | `DispatcherServlet`을 중심으로 요청을 처리하는 Spring의 웹 프레임워크 (MVC 패턴 적용)        |
| **3-Layer 아키텍처** | 애플리케이션을 역할에 따라 Controller, Service, Repository로 나누는 구조로 유지보수성과 테스트성 향상 |
