---
layout: post
title: Programmers [07]
subtitle: 프로그래머스 문제풀이
description: "프로그래머스 문제풀이"
categories: programmers
tags: [Programmers, Lv.1]
---

## 프로그래머스 문제풀이 
### 하샤드 수

### 문제 설명
양의 정수 x가 하샤드 수이려면 x의 자릿수의 합으로 x가 나누어져야 합니다.   
예를 들어 18의 자릿수 합은 1+8=9이고, 18은 9로 나누어 떨어지므로 18은 하샤드 수입니다.    
자연수 x를 입력받아 x가 하샤드 수인지 아닌지 검사하는 함수, solution을 완성해주세요.   

### 입출력 예

| x  | return |
|----|--------|
| 10 | true   |
| 12 | true   |
| 11 | false  |
| 13 | false  |

### 입출력 예 설명

입출력 예 #1
10의 모든 자릿수의 합은 1입니다. 10은 1로 나누어 떨어지므로 10은 하샤드 수입니다.   

입출력 예 #2
12의 모든 자릿수의 합은 3입니다. 12는 3으로 나누어 떨어지므로 12는 하샤드 수입니다.   

입출력 예 #3
11의 모든 자릿수의 합은 2입니다. 11은 2로 나누어 떨어지지 않으므로 11는 하샤드 수가 아닙니다.   

입출력 예 #4
13의 모든 자릿수의 합은 4입니다. 13은 4로 나누어 떨어지지 않으므로 13은 하샤드 수가 아닙니다  

---

답안

```java
class Solution {
    public boolean solution(int x) {
        boolean answer = true;
        int num = x;
        int sum = 0;
    
        while(num > 0) {
            sum += num % 10;
            num /= 10;
        }
        
        if(x % sum != 0) {
            answer = false;
        }
        
        return answer;
    }
}
```

boolean 함수가 뭔지 알아보자
boolean type는 1byte 크기로 결과가 참이면 true를, 거짓이면 false를 주는 변수이다.
 
먼저 while문을 보면 10으로 나눈 값을 가져가면 맨 뒷자리 인 값을 가져갈 수 있다.   
그리고 if문에 들어가 sum이 0이 아닐경우 false를 출력하게 되어 문제를 해결할 수 있다.

---

### 후기
기본적인 문법과 변수를 더 공부해야 할거같다.