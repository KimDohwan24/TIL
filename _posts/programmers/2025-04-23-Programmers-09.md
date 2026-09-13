---
layout: post
title: Programmers [09]
subtitle: 프로그래머스 문제풀이
description: "프로그래머스 문제풀이"
categories: programmers
tags: [Programmers, Lv.1]
---

## 정수 내림차순으로 배치하기

### 문제설명
함수 solution은 정수 n을 매개변수로 입력받습니다. n의 각 자릿수를 큰것부터 작은 순으로 정렬한 새로운 정수를 리턴해주세요. 예를들어 n이 118372면 873211을 리턴하면 됩니다.

### 제한조건 
- n은 1이상 8000000000 이하인 자연수입니다.

### 입출력 예 

| n      | return |
|--------|--------|
| 118372 | 873211 |

### 주어진 솔루션

```java
class Solution {
    public long solution(long n) {
        long answer = 0;
        
        return answer;
    }
}
```

---

### 문제 풀이

일단 솔루션을 보면 long타입으로 받았기 때문에 String형으로 바꿔 split을 해 배열에 담아둔 뒤 내림차순을 하면 될거같다.

```java
        String[] arr = String.valueOf(n).split("");

        // 확인용
//        for(String a : arr){
//            System.out.println("a = " + a);
//        }

        // 배열에 내림차순으로 정렬
        Arrays.sort(arr, Collections.reverseOrder());

        // 한 배열로 담기
        StringBuilder stringBuilder = new StringBuilder();
        for (String s : arr) {
            stringBuilder.append(s);
        }
```

### Split 
- 입력받은 정규 표현식 또는 특정 문자를 기준으로 문자열을 나누어 배열에 정리하여 리턴하는 함수
- 위에선 ("")으로 나눴으니 한글자씩 따로따로 나누어 배열에 정리한다.

### 해답

```java
import java.util.Arrays;
import java.util.Collections;

class Solution {
    public long solution(long n) {
        long answer = 0;
        String[] arr = String.valueOf(n).split("");
        
        Arrays.sort(arr, Collections.reverseOrder());
        
        StringBuilder stringBuilder = new StringBuilder();
        
        for(String s : arr){
            stringBuilder.append(s);
        }
        return Long.parseLong(stringBuilder.toString());
    }
}
```

- 이런식으로 풀면 풀린다.

### 다른사람 풀이

```java
public class ReverseInt {
    String res = "";
    public int reverseInt(int n){
        res = "";
        Integer.toString(n)
        .chars()
        .sorted()
        .forEach(c -> res = Character.valueOf((char)c) + res);
        return Integer.parseInt(res);
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void  main(String[] args){
        ReverseInt ri = new ReverseInt();
        System.out.println(ri.reverseInt(118372));
    }
}
```

---
### 느낀점 

- lambda식으로 표기하면 저렇게 짧게 표현되는 것을 알 수 있었다. 나도 lambda와 좀 더 친해져보도록 노력해야겠다.