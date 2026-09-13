---
layout: post
title: Programmers [08]
subtitle: 프로그래머스 문제풀이
description: "프로그래머스 문제풀이"
categories: programmers
tags: [Programmers, Lv.1]
---

## 프로그래머스 문제풀이 
### K번째 수

---

### 문제 설명
배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
3. 2에서 나온 배열의 3번째 숫자는 5입니다.   

배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 입출력 예

| array                  | commands                          | return     |
|------------------------|-----------------------------------|------------|
| [1, 5, 2, 6, 3, 7, 4]  | [ [2, 5, 3], [4, 4, 1], [1, 7, 3] ] | [5, 6, 3]  |

### 입출력 예 설명
[1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.
[1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.
[1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.

---

```java

import java.util.Arrays;
class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = new int[commands.length];

        for(int i=0; i<commands.length; i++){
            int[] temp = Arrays.copyOfRange(array, commands[i][0]-1, commands[i][1]);
            Arrays.sort(temp);
            answer[i] = temp[commands[i][2]-1];
        }

        return answer;
    }
}

```
### 답안

이번 문제는 도저히 모르겠어서 정답을 보면서 해석해보았다.   
문제를 해석하면서 copyOfRange() 함수를 알게 되었는데,     
Arrays.copyOfRang(복사할 원본 배열, 복사를 시작할 인덱스, 복사를 끝낼 인덱스)   
-> 지정한 배열에서 특정 범위만큼의 요소들을 복사해 새로운 배열로 반환한다. 복사할 배열의 길이가 복사를 끝낼 인덱스로 입력한 길이보다 작을 경우 원본 배열의 마지막 인덱스 이후의 값은 배열의 타입 기본값으로 초기화되어 copy 된다.

```java
int[] intArr = new int[] {1, 2, 3, 4, 5};
int[] intArrCopy = Arrays.copyOfRange(intArr, 2, 4);
for(int i : intArrCopy) System.out.println(i);

// 출력
// 3
// 4
```

이런식으로 사용하는 함수도 있다.

