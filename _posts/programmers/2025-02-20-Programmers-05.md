---
layout: post
title: Programmers [05]
subtitle: 프로그래머스 문제풀이
description: "프로그래머스 문제풀이"
categories: programmers
tags: [Programmers, Lv.1]
---

## 프로그래머스 문제풀이 
### 숫자 문자열과 영단어

---

### 문제설명

네오와 프로도가 숫자놀이를 하고 있습니다. 네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.

다음은 숫자의 일부 자릿수를 영단어로 바꾸는 예시입니다.

* 1478 → "one4seveneight"
* 234567 → "23four5six7"
* 10203 → "1zerotwozero3"

이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 s가 매개변수로 주어집니다. s가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.

참고로 각 숫자에 대응되는 영단어는 다음 표와 같습니다.

| 숫자 | 영단어 |
|----|--------|
| 0  | zero   |
| 1  | one    |
| 2  | two    |
| 3  | three  |
| 4  | four   |
| 5  | five   |
| 6  | six    |
| 7  | seven  |
| 8  | eight  |
| 9  | nine   |

### 제한사항

* 1 ≤ s의 길이 ≤ 50
* s가 "zero" 또는 "0"으로 시작하는 경우는 주어지지 않습니다.
* return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 s로 주어집니다.

### 입출력 예

| s                | result  |
|-----------------|---------|
| "one4seveneight" | 1478    |
| "23four5six7"   | 234567  |
| "2three45sixseven" | 234567 |
| "123"           | 123     |

### 입출력 예 설명

입출력 예 설명
* 입출력 예 #1

문제 예시와 같습니다.
* 입출력 예 #2

문제 예시와 같습니다.
* 입출력 예 #3

* "three"는 3, "six"는 6, "seven"은 7에 대응되기 때문에 정답은 입출력 예 #2와 같은 234567이 됩니다.
* 입출력 예 #2와 #3과 같이 같은 정답을 가리키는 문자열이 여러 가지가 나올 수 있습니다.

입출력 예 #4
* s에는 영단어로 바뀐 부분이 없습니다.

> 주어진 솔루련
```java
class Solution {
    public int[] solution(int[] numbers) {
        int[] answer = {};
        return answer;
    }
}
```

---

문자를 숫자로 바꾸는 함수가 필요하다.
찾아보니 replace 라는 함수가 존재한다.

### replace
```java
String a = "무궁화 삼천리 화려강산 대한사람 대한으로 길이 보전하세 ";	
//replace([기존문자],[바꿀문자])
a= a.replace("대한", "민국");	
System.out.println(a);

//결과값 : 무궁화 삼천리 화려강산 민국사람 민국으로 길이 보전하세
```

이런식으로 사용하는거 같다.
먼저 영단어를 숫자로 바꿀거기 때문에 묶어두고 사용하면 될거같다.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int solution(String s) {
        int answer = 0;
        
        Map<String, String> replacements = new HashMap<>();
        replacements.put("zero","0");
        replacements.put("one","1");
        replacements.put("two","2");
        replacements.put("three","3");
        replacements.put("four","4");
        replacements.put("five","5");
        replacements.put("six","6");
        replacements.put("seven","7");
        replacements.put("eight","8");
        replacements.put("nine","9");

        for(Map.Entry<String,String> entry : replacements.entrySet()){
            s = s.replace(entry.getKey(), entry.getValue());
        }
        answer = Integer.parseInt(s);
        
        return answer;
    }
}
```

나는 이런식으로 먼저 바꿔줄 단어들을 묶어두고 나서 한번에 바꿔주도록 만들었다.

### 후기 
처음보는 함수가 너무 많은거 같다. 하나하나 써보면서 익힐 필요가 있을거같다.