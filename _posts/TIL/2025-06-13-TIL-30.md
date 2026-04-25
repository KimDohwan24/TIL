---
layout: post
title: JPQL
subtitle: Java Persistence Query Language
categories: TIL
tags: [TIL]
---

## JPQL 
- JPA는 SQL을 추상화한 JPQL이라는 개체 지향 쿼리 언어를 제공한다
- 테이블을 대상으로 쿼리 하는 것이 아닌 엔티티 객체를 대항으로 쿼리한다
- JPQL은 SQL을 추상화했기 때문에 특정 데이터베이스 SQL에 의존하지 않는 장점이 있다
- JPQL은 SQL과 문법이 유사하며, SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN을 지원한다
- `JPQL은 결국 SQL로 변환된다`

### JPQL 예제

```java

// 예: Member 엔티티에서 이름이 '홍길동'인 멤버 조회
@Query("SELECT m FROM Member m WHERE m.name = :name")
List<Member> findByName(@Param("name") String name);

```

- 위 쿼리에서 `Member`는 자바 엔티티 클래스이고, `name`은 필드명입니다.
- SQL에서는 `SELECT * FROM member WHERE name = '홍길동'`이지만, JPQL에서는 객체 중심으로 표현한다.

### JPQL 사용 방식

1. `@Query` 어노테이션 사용

```java

@Query("SELECT m FROM Member m WHERE m.name = :name")
List<Member> findByName(@Param("name") String name);

```

2. `EntityManager` 직접 사용

```java

TypedQuery<Member> query = em.createQuery("SELECT m FROM Member m", Member.class);
List<Member> resultList = query.getResultList();

```

### TypedQuery, Query
- JPQL을 실행하려면 쿼리 객체를 만들어야한다. 쿼리 객체로는 TypedQuery와 Query가 있는데,    
반환할 타입을 명확하게 지정할 수 있으면 TypedQuery와 객체를, 명확하게 지정할 수 없으면 Query 객체를 사용한다

#### TypedQuery

```java

public static void typedQuery(EntityManager em) {    
    String jpql = "select m from Member m";	
    TypedQuery<Member> query = em.createQuery(jpql, Member.class);		
    List<Member> list = query.getResultList();	
    for( Member member : list) {		
        System.out.println("Member : " + member);	
        }
    }

```
- EntityManager 객체에서 createQuery() 메소드를 호출하면 쿼리가 생성된다.
- em.createQuery 메소드를 호출할 때 두 번째 인자로 엔티티 클래스를 넘겨준다.

#### Query

```java
public static void Query(EntityManager em) {   
    String jpql = "select m.name, m.age from Member m";	
    Query query = em.createQuery(jpql);		
    List<Object> list = query.getResultList();	   
    for( Object object : list ) {
        	Object[] results = (Object[]) object;	      	      
                for( Object result : results ) {	          
                    System.out.print ( result );	     
                }	     
            System.out.println();	  
        }
    }
```
- Query 타입은 데이터 검색 결과의 타입을 명시하지 않는다.

### 파라미터 바인딩
- 파라미터 바인딩에는 이름 기준 파라미터와 위치 기준 파라미터가 있다.
- 위치 기준 파라미터 보다 이름 기준 파라미터가 더 명확하다.

#### 이름 기준 파라미터 

```java
public static void namedParameter(EntityManager em, String param) {    
    String jpql = "select m from Member m where m.name = :name";	
    TypedQuery<Member> query = em.createQuery(jpql, Book.class);	
    query.setParameter("name", param);		
    List<Member> list = query.getResultList();
    }
```
- 이름을 기준으로 파라미터를 바인딩 한다. 콜론( : ) 을 사용해 데이터가 추가될 곳을 지정하고,
query.setParameter() 메소드를 호출해 데이터를 동적으로 바인딩 한다.

#### 위치 기준 파라미터
- 엔티티를 대상을 조회하려면 편리하겠지만, 꼭 필요한 데이터들만 선택해서 조회해야 할 때도 있다.
- 이럴때 MemberDto 처럼 의미있는 객체로 변환해서 사용한다.

```java

@AllArgsConstructor
@NoArgsConstructor
@Getter 
@Setter
public class MemberDto {
    	private String name;	private int age;
    }

```

``` java

public static void useDto (EntityManager em) { 
    // Dto 사용 ( new 명령어 )	
    String jpql = "select new com.coco.example.MemberDto(m.name, m.age) from Member m";	
    TypedQuery<MemberDto> query = em.createQuery(jpql, MemberDto.class);		
    List<MemberDto> list = query.getResultList();	
    for( BookDTO dto : list) {		
        System.out.println("dto : " + dto);	
        }
    }

```

- select 와 from 사이에 new 라는 키워드 뒤에 Dto의 패키지명까지 작성해야 한다.
- 이 때 new는 객체를 생성하라는 의미가 아니라 JPQL에서 지원하는 new 키워드이다.

### JPQL vs SQL

| 항목    | JPQL                | SQL           |
| ----- | ------------------- | ------------- |
| 대상    | 엔티티(Entity), 필드명    | 테이블, 컬럼명      |
| 반환 타입 | 객체(Entity), 필드, DTO | Row 데이터       |
| 실행 대상 | JPA (영속성 컨텍스트)      | 데이터베이스        |
| 목적    | 객체 중심의 비즈니스 로직 처리   | DB 중심의 데이터 처리 |
| JOIN  | 객체 필드로 조인           | 외래 키를 기준으로 조인 |

### JPQL 주의사항
1. 테이블명 대신 클래스명, 컬럼명 대신 필드명을 사용
2. SELECT 필드명은 항상 엔티티 기준
3. 엔티티가 아닌 테이블을 직접 조회할 수는 없음
4. 엔티티 매핑이 잘못돼 있으면 쿼리가 실패


