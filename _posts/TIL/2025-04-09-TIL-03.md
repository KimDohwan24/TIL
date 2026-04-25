---
layout: post
title: SQL 기본기 다지기
subtitle: SQL
categories: TIL
tags: [TIL , SQL]
---

# SQL 구문 배우기

## SQL 기본 틀

```sql
select * <- 전체 조회 
from table_테이블 이름
where 조건
group by (뭘로 그룹지을건지)
order by (데이터를 출력하거나 정렬하기)
```

1) 문자 변경

- REPLACE : 지정한 문자를 다른 문자로 변경
- SUBSTRING : 특정 문자만 추출
- CONCAT : 여러 문자를 합하여 포멧팅   

2) 조건문 
- IF : if(조건, 조건을 충족할때, 조건을 충족하지 못할때)

```sql
- CASE WHEN END : 
  case when 조건1 then 값(수식)1
       when 조건2 then 값(수식)2
       else 값(수식)3
    end
```

## Subquery (서브쿼리)

Subquery가 필요한 경우
- 여러번 연산을 수행해야 할 때 
ex) 수수료를 부과할 수 있는 시간을 구하고
구해진 시간에 주문 금액별로 가중치를 주고
가중치를 적용한 결과로 최종 예상 배달비를 계산할 때   
   
- 조건문에 연산 결과를 사용해야 할 때
ex) 음식 타입별 평균 음식 주문금액 따라 음식 상/중/하 를 나누고 싶을 때   
   
- 조건에 Query 결과를 사용하고 싶을 때
ex) 30대 이상이 주문한 결과만 조회하고 싶을때    

## Join

1) Join이 필요한 경우
- 주문 가격은 주문테이블에 있지만, 어떤 수단으로 결제를 했는지는 결제테이블에 있다
- 주문을 한 사람을 확인하려면, 주문 테이블과 고객 테이블에서 각각 정보를 가져와서 엑셀에서 합쳐줘야해요 
- 주문 건별 수수료를 계산하려면 수수료율 이 필요한데, 결제 테이블에 있어서 어떻게 연산할 수 있을지 모르겠어요 

2) Join의 기본 원리와 종류
- 액셀에서 vlookup(고객ID , 고객정보 , 3 , false) 과 비슷하다   

## LEET JOIN 
- 공통 컬럼 (키값) 을 기준으로, 하나의 테이블에 값이 없더라도 모두 조회되는 경우를 의미한다.

## INNER JOIN 
- 공통 컬럼(키값) 을 기준으로, 두 테이블 모두에 있는 값만 조회합니다.  

## IS NOT NULL
- null값 제외하기

## IS NULL
- null값만 출력하기

## Pivot table 
- 2개 이상의 기준으로 데이터를 집계할 때, 보기 쉽게 배열하여 보여주는 것을 의미

## WINDOW 함수
- 각 행의 관계를 정의하기 위한 함수로 그룹 내의 연산을 쉽게 만들어준다.

## WINDOW 함수 종류
- window_function : 기능 명을 사용해줍니다 ( sum , avg와 같이 기능명이 있다.)   
- argument : 함수에 따라 작성하거나 생략한다.   
- partition by : 그룹을 나누기 위한 기준이다.
- order by : window function을 적용할 때 정렬 할 컬럼 기준을 적어준다.

