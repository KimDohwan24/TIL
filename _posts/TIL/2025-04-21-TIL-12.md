---
layout: post
title: Generic
subtitle: Generic
categories: TIL
tags: [TIL]
---

## Generic 
- 클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법을 의미
- 제네릭을 활용하면 코드 재사용성과 타입 안정성을 보장받을 수 있다.
- 사용방법 List< 타입 매개변수 > -> List<String> stringList = new ArrayList<String>();


### 주의사항! 
- 제네릭 타입 자체로 타입을 지정하여 객체를 생성하는 것은 불가능 한다.   
즉, new 연산자 뒤에 제네릭 타입 파라미터가 올수는 없다.
-  static 변수의 데이터 타입으로 제네릭 타입 파라미터가 올수는 없다. 

### Generic 예제

```java 
    // Main Class
    // 1. 재사용성 보장 (타입 소거 : T -> Object)
    GenericBox<String> strGBox = new GenericBox<String>("ABC");
    GenericBox<Integer> intGBox = new GenericBox<Integer>(100);
    GenericBox<Double> doubleGBox = new GenericBox<Double>(0.1);

    // 2. 타입 안정성 보장(타입 소거 : 자동으로 down casting 선언)
    String strGBoxItem = strGBox.getItem();
    System.out.println("strGBoxItem = " + strGBoxItem);

    int intGBoxItem = intGBox.getItem();
    System.out.println("intGBoxItem = " + intGBoxItem);

    double doubleGBoxItem = doubleGBox.getItem();
    System.out.println("doubleGBoxItem = " + doubleGBoxItem);
```

```java
    // GenericBox class 
    public class GenericBox<T> {
    // 속성
    private T item;

    // 생성자
    public GenericBox(T item){
        this.item = item;
    }

    // 기능
    public T getItem(){
        return this.item;
    }

    // 일반 메서드
    public  void printItem(T item){
        System.out.println(item);
    }
}

```

### 제네릭 메서드(Generic Method)
- 제네릭은 클래스, 메서드 등에 사용되는 <T>타입 매개변수를 의미한다
- 타입을 미리 지정하지 않고 사용 시점에서 유연하게 결졍할 수 있는 문법이다.

```java
    public class GenericBox<T> {
    // 속성
    private T item;

    // 생성자
    public GenericBox(T item){
        this.item = item;
    }

    // 기능
    public T getItem(){
        return this.item;
    }

    // 일반 메서드
    public  void printItem(T item){
        System.out.println(item);
    }

    // 제네릭 메서드
    public <S> void printBoxItem(S item){
        System.out.println();
    }
}
// 위의 <T>(클래스 제네릭 타입)와 <S>(제네릭 메서드)는 독립적인 타입 매개변수를 가진다. 
```

---

```java
 // 제네릭 활용
        // 1. 재사용성 보장 (타입 소거 : T -> Object)
        GenericBox<String> strGBox = new GenericBox<String>("ABC");
        GenericBox<Integer> intGBox = new GenericBox<Integer>(100);
        GenericBox<Double> doubleGBox = new GenericBox<Double>(0.1);

        // 2. 타입 안정성 보장(타입 소거 : 자동으로 down casting 선언)
        String strGBoxItem = strGBox.getItem();
        System.out.println("strGBoxItem = " + strGBoxItem);

        int intGBoxItem = intGBox.getItem();
        System.out.println("intGBoxItem = " + intGBoxItem);

        double doubleGBoxItem = doubleGBox.getItem();
        System.out.println("doubleGBoxItem = " + doubleGBoxItem);

        // 일반 메서드 (String 기준으로 타입 소거 발생)
        strGBox.printBoxItem("ABC");
//        strGBox.printItem(100);
//        strGBox.printItem(0.1);

        // 제네릭 메서드 (String과 상관 없음)
        strGBox.printBoxItem("ABC");
        strGBox.printBoxItem(100);
        strGBox.printBoxItem(0.1);

        // 결과값
//         item = ABC
//         strGBoxItem = ABC
//         intGBoxItem = 100
//         doubleGBoxItem = 0.1

```

