---
layout: post
title: Programmers [01]
subtitle: 프로그래머스 문제풀이
description: "프로그래머스 문제풀이"
categories: programmers
tags: [Programmers, Lv.2]
---

## 프로그래머스 문제풀이 
### 뒤에 있는 큰 수 찾기  
---

### 문제 설명 

정수로 이루어진 배열 numbers가 있습니다. 배열 의 각 원소들에 대해 자신보다 뒤에 있는 숫자 중에서 자신보다 크면서 가장 가까이 있는 수를 뒷 큰수라고 합니다.
정수 배열 numbers가 매개변수로 주어질 때, 모든 원소에 대한 뒷 큰수들을 차례로 담은 배열을 return 하도록 solution 함수를 완성해주세요. 단, 뒷 큰수가 존재하지 않는 원소는 -1을 담습니다.

### 입출력 예

| numbers          | result         |
|-----------------|---------------|
| [2, 3, 3, 5]    | [3, 5, 5, -1] |
| [9, 1, 5, 3, 6, 2] | [-1, 5, 6, 6, -1, -1] |

### 입출력 예 설명

입출력 예 #1
2의 뒷 큰수는 3입니다. 첫 번째 3의 뒷 큰수는 5입니다. 두 번째 3 또한 마찬가지입니다. 5는 뒷 큰수가 없으므로 -1입니다. 위 수들을 차례대로 배열에 담으면 [3, 5, 5, -1]이 됩니다.

입출력 예 #2
9는 뒷 큰수가 없으므로 -1입니다. 1의 뒷 큰수는 5이며, 5와 3의 뒷 큰수는 6입니다. 6과 2는 뒷 큰수가 없으므로 -1입니다. 위 수들을 차례대로 배열에 담으면 [-1, 5, 6, 6, -1, -1]이 됩니다.


> 주어진 솔루션

```java
class Solution {
    public int[] solution(int[] numbers) {
        int[] answer = {};
        return answer;
    }
}
```
---

문제를 살펴보면 스택을 이용해야 할거같다.

Stack은 후입선출 구조로 이루어져 있는 자료구조이다.

### Stack

스택에서 재공하는 연산 종류
* **pop()** : 스택에서 가장 위에 있는 항목을 제거
* **push(item)** : item 하나를 스택의 가장 윗 부분에 추가
* **peek()** : 스택의 가장 위에 있는 항복을 반환
* **isEmpty()** : 스택이 비어 있을때 **true**를 반환
* **push(data)** : 데이터가 가득 차 있는지 확인 후 가득 차 있지 않다면 데이터를 추가 

이것을 이용하여 문제를 해결해보자


```java
import java.util.*;

class Solution {
    public int[] solution(int[] numbers) {
        int[] answer = new int[numbers.length];
		Arrays.fill(answer, -1); 
        	// fill 함수는 배열을 for문을 사용하지 않고 초기화 할때 사용한다
		// { -1,-1,-,1 ~ } 

		Stack<Integer> stack = new Stack<>();

		for (int i = 0; i < numbers.length; i++) {
			while (!stack.isEmpty() && numbers[i] > numbers[stack.peek()]) {
				answer[stack.pop()] = numbers[i];
			}
			
			stack.push(i);
		}

		return answer;
    }
}
```

이렇게 풀면 될것같다.

### 후기
아직 Lv.2 문제는 찾지 않으면 풀리지 않지만 계속 연습하다 보면 풀릴 날이 올 것이다.