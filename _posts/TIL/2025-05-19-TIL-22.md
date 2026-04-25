---
layout: post
title: Spring Bean, Singleton Pattern
subtitle: Bean, Singleton
categories: TIL
tags: [TIL]
---

## Bean 이란?
- 빈( Bean )은 스프링 컨테이너에 의해 관리되는 재사용 가능한 소프트웨어 컴포넌트이다.
- 즉 스프링 컨테이너가 관리하는 자바 객체를 뜻하며, 하나 이상의 빈(Bean)을 관리한다

### 스프링 컨테이너?
- 스프링 컨테이너는 스프링 빈의 생명 주기를 관리하며, 생성된 스프링 빈들에게 추가적인 기능을 제공하는 역할을 한다.
- IoC와 DI의 원리가 스프링 컨테이너에 적용된다.

### Bean 핵심 개념

| 용어               | 설명                                     |
| ---------------- | -------------------------------------- |
| **Bean**         | Spring 컨테이너가 생성하고 관리하는 객체              |
| **IoC (제어의 역전)** | 객체 생성과 의존성 주입을 개발자가 아닌 Spring 컨테이너가 담당 |
| **DI (의존성 주입)**  | Bean 간의 의존 관계를 자동으로 주입해주는 메커니즘         |

### Bean 등록 방식
1. 어노테이션 기반 등록 ( 주로 이 방법을 많이 사용 )   

| 어노테이션         | 대상     | 역할                                  |
| ------------- | ------ | ----------------------------------- |
| `@Component`  | 일반 클래스 | 컴포넌트 스캔 대상                          |
| `@Service`    | 서비스 계층 | 의미적으로 서비스임을 나타냄 (`@Component`의 특수화) |
| `@Repository` | DAO 계층 | DB 예외를 Spring 예외로 변환                |
| `@Controller` | 웹 컨트롤러 | HTTP 요청 처리 (Spring MVC 전용)          |

- 이 어노테이션들이 붙은 클래스는 Spring이 자동적으로 Bean 에 등록한다
- 스캔 대상은 `@Component` 범위 안이어야 한다.

2. 자바 설정 클래스 (`@Configuration`)

```java
@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

- 이 경우 `@Bean` 메서드의 리턴 객체가 Spring bean에 등록된다.

### Bean 사용 ( 의존성 주입 )
- Bean을 사용하는 곳에서는 다음 방식으로 주입할 수 있다.

```java
@Autowired
private MyService myService;

-------------------------------------

private final MyService myService;

@Autowired
public MyController(MyService myService) {
    this.myService = myService;
}
```

### Spring Bean 요약

| 개념          | 설명                                                              |
| ----------- | --------------------------------------------------------------- |
| Spring Bean | Spring 컨테이너가 생성하고 관리하는 객체                                       |
| 등록 방식       | `@Component`, `@Service`, `@Repository`, `@Controller`, `@Bean` |
| 관리 이유       | 객체 생명주기 관리, DI를 통한 유연한 설계, 테스트 용이성                              |


## Singleton Pattern
- 싱글톤은 객체를 단 하나만 생성해서 공유하도록 보장하는 디자인 패턴이다
- 프로그램이 실행되는 동안 한 클래스에 대해 오직 하나의 인스턴스만 존재하도록 하는 방식이다

### 그럼 싱글톤은 왜쓸까?
1. 메모리 절약 ( 여러개를 만들지 않음 )
2. 객체 간 공유가 필요할 때 사용 ( 서브시, 설정 ... )
3. 성능 향상 ( 객체 생성 비용 감소 )

### 일반적인 싱글톤 구현 
```java
public class MySingleton {
    private static final MySingleton instance = new MySingleton();

    private MySingleton() {
        // private 생성자 → 외부에서 new로 생성 불가
    }

    public static MySingleton getInstance() {
        return instance;
    }
}
```

### Spring과 싱글톤
- Spring은 기본적으로 `모든 Bean을 싱글톤으로 관리`한다.
- `@Component`,`@Service`,`@Repository` 등을 붙여 Bean으로 등록하면 자동으로 하나의 인스턴스만 생성되고, 필요한 곳에 주입해서 재사용한다.

```java
@Service
public class UserService {
    // Spring은 이 클래스를 싱글톤으로 관리함
}

---------------------------------------------

@Autowired
private UserService userService1;

@Autowired
private UserService userService2;

// 두 필드는 같은 인스턴스를 참조함
```

### 싱글톤 요약

| 항목          | 설명                                     |
| ----------- | -------------------------------------- |
| 싱글톤 패턴      | 애플리케이션에 하나의 인스턴스만 존재하게 만드는 디자인 패턴      |
| Spring Bean | 기본적으로 싱글톤으로 생성되고 관리됨                   |
| 장점          | 메모리 절약, 공유 용이성, 성능 향상                  |
| 유의사항        | 상태를 가지는 싱글톤 객체는 동시성 문제 주의 (스레드 안전성 필요) |
