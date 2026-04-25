---
layout: post
title: AOP
subtitle: 관점지향 프로그래밍( Aspect - Oriented Programming )
categories: TIL
tags: [TIL, AOP]
---

## AOP ( 관점 지향 프로그래밍 )
- AOP는 OOP( Object Oriented Programming )를 돕는 보조적인 기술로, 관심사의 분리( 기능의 분리 )의 문제를 해결하기 위해 만들어진 프로그래밍 패러다임이다.
- AOP는 기능을 핵심 관심 사항( Core Concern )과 공통 관심 사항( Cross - Cutting Concern )으로 분리시키고 각각을 모듈화 하는것을 의미한다.

### AOP의 핵심 개념

1. 관심사 ( Concern )
- 애플리케이션에서 어떤 기능이나 책임을 말한다
- 예 : 비즈니스 로직, 보안, 로깅 등..

2. 횡단 관심사 ( Cross - Cutting Concern )
- 여러 모듈에서 공통적으로 사용되는 기능
- 예 : 로그출력, 트랜잭션 처리, 인증/인가 등..

3. Aspect
- 횡단 관심사를 모듈화한 단위
- 예 : 로깅 기능을 `LoggingAspect`라는 클래스로 작성

### AOP의 주요 용어

| 용어             | 설명                                               |
| -------------- | ------------------------------------------------ |
| Join Point | Advice가 적용될 수 있는 지점. 메서드 실행, 예외 발생 등.            |
| Advice    | 실제로 수행할 작업(로직). 예: 메서드 실행 전/후에 로그 찍기.            |
| Pointcut   | Advice를 적용할 Join Point를 결정하는 표현식.                |
| Aspect     | Advice + Pointcut을 합쳐서 정의한 클래스.                  |
| Weaving    | Aspect를 실제 코드에 적용하는 과정. (컴파일 시, 클래스 로딩 시, 런타임 등) |

### AOp 클래스 정의 하는 방법

```java

// 밑의 두개의 어노테이션을 작성하면 된다.
@Aspect // AOP 클래스임을 명시하는 어노테이션
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void beforeMethod(JoinPoint joinPoint) {
        System.out.println("Before: " + joinPoint.getSignature().getName());
    }

    @AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", returning = "result")
    public void afterMethod(JoinPoint joinPoint, Object result) {
        System.out.println("After: " + joinPoint.getSignature().getName() + " Result: " + result);
    }
}


```

### Advice 종류 요약   

| 종류                | 설명                             |
| ----------------- | ------------------------------ |
| `@Before`         | 메서드 실행 전에 실행                   |
| `@AfterReturning` | 메서드가 정상적으로 종료된 후 실행            |
| `@AfterThrowing`  | 예외 발생 후 실행                     |
| `@After`          | 정상 종료든 예외든 무조건 실행 (finally 역할) |
| `@Around`         | 메서드 실행 전후를 모두 제어 (가장 강력하고 유연함) |

### AOP의 장점과 주의점

- 장점
1. 코드 중복 제거
2. 모듈화 개선
3. 유지보수 용이
4. 핵심 로직과 부가 로직 분리 가능 ( 재사용성을 높임 )

- 주의점
1. 너무 남용하면 로직 추적이 어려워짐
2. Spring AOP는 기본적으로 프록시 기반이므로 인터페이스 기반 또는 클래스 기반 프록시임을 이해하고 사용해야함
3. private 매서드에는 적용 X ( Spring AOP 기준 )

## 프록시(Proxy)
- 다른 객체에 대한 접근을 제어하는 대리자이다.
- 즉, 클라이언트는 진짜 객체 대신 프록시 객체를 사용하고, 프록시가 진짜 객체에 요청을 전달하면서 추가기능 ( 로깅, 보안, 트랜잭션 등 )을 수행할 수 있다.
