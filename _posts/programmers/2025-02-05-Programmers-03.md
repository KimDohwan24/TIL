---
layout: post
title: Programmers [03]
subtitle: 프로그래머스 문제풀이
description: "프로그래머스 문제풀이"
categories: programmers
tags: [Programmers, Lv.0]
---

## 프로그래머스 문제풀이
### [PCCE 병과분류]

---

### 문제설명

퓨쳐종합병원에서는 접수한 환자가 진료받을 병과에 따라 자동으로 환자 코드를 부여해 주는 프로그램이 있습니다. 환자 코드의 마지막 네 글자를 보면 환자가 어디 병과에서 진료를 받아야 할지 알 수 있습니다. 
예를 들어 환자의 코드가 "_eye"로 끝난다면 안과를, "head"로 끝난다면 신경외과 진료를 보게 됩니다. 환자 코드의 마지막 글자에 따른 병과 분류 기준은 다음과 같습니다.

| 마지막 글자 | 병과           |
|------------|----------------|
| "_eye"     | Ophthalmology  |
| "head"     | Neurosurgery   |
| "infl"     | Orthopedics    |
| "skin"     | Dermatology    |

환자의 코드를 나타내는 문자열 code를 입력받아 위 표에 맞는 병과를 출력하도록 빈칸을 채워 코드를 완성해 주세요. 위 표의 단어로 끝나지 않는다면 "direct recommendation"를 출력합니다.

### 제한사항

* 4 ≤ code의 길이 ≤ 20
* code는 영어 소문자와 숫자, 언더바("_")로 이루어져 있습니다.

### 입출력 예
입력 #1
> dry_eye

출력 #1
> Ophthalmologyc

입력 #2
> pat23_08_20_head

출력 #2
> Neurosurgery

### 입출력 예 설명

입출력 예 #1
* code가 "_eye"로 끝나기 때문에 "Ophthalmologyc"를 출력합니다.

입출력 예 #2
* code가 "head"로 끝나기 때문에 "Neurosurgery"를 출력합니다.

> 주어진 솔루션 
```java
import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String code = sc.next();
        String lastFourWords = code.substring(code.length()-4, code.length());

        if(lastFourWords.equals( [빈 칸] )){
            System.out.println("Ophthalmologyc");
        }
        else if([빈 칸]){
            System.out.println("Neurosurgery");
        }
        else if([빈 칸]){
            System.out.println("Orthopedics");
        }
        else if([빈 칸]){
            System.out.println("Dermatology");
        }
        [빈 칸]{
            System.out.println("direct recommendation");
        }
    }
}
```

---

### equals()와 비교연산자(==) 차이점
equals()가 들어가 있는걸 보면 equals() 메서드가 어떻게 쓰이는지 알아봐야 한다.

간단한 예제로 보자면

```java 
package joon;

public class codeTest {

    public static void main(String[] args) throws Exception{

        String str1 = "abc";
        String str2 = str1;
        String str3 = new String("abc");

        // == 연산자는 주소를 비교합니다.
        System.out.println(str1 == str2); // true
        // str2 에 st1 값을 넣었으므로 주소를 같이 공유합니다.

        System.out.println(str1 == str3); // false
        // str1 과 str3는 각각 생성 되었으므로 주소가 다릅니다.

        // equals() 는 내용을 비교합니다.
        System.out.println(str1.equals(str2)); // ture
        System.out.println(str1.equals(str3)); // true
        // 내용을 비교하기떄문에 abc 내용이 같으므로 true 가 반환됩니다.

    }
}
```

이런 식으로 사용할 수 있는데
== 비교연산자와 equals() 메서드의 차이점이 있다.

== 비교연산자는 주소의 값을 비교할 때 사용한다. 
즉 실제 내용의 값이 아닌 자료의 위치의 값을 비교하는 것이다.

equals() 메소드는 객체끼리 내용을 비교할 때 사용한다.

이제 문제를 풀어보자면 
```java
import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String code = sc.next();
        String lastFourWords = code.substring(code.length()-4, code.length());

        if(lastFourWords.equals("_eye")){
            System.out.println("Ophthalmologyc");
        }
        else if(lastFourWords.equals("head")){
            System.out.println("Neurosurgery");
        }
        else if(lastFourWords.equals("infl")){
            System.out.println("Orthopedics");
        }
        else if(lastFourWords.equals("skin")){
            System.out.println("Dermatology");
        }
        else{
            System.out.println("direct recommendation");
        }
    }
}
```

이렇게 답이 나온다.

### 후기
기초적인 함수라도 다시한번 정확히 집고 넘어가야 나중에 정확하게 사용할 수 있다.