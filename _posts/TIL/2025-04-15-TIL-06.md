---
layout: post
title: 객체지향 이해하기 - 2
subtitle: Encapsulation, extands, Overriding
categories: TIL
tags: [TIL]
---

## 캡슐화 ( Encapsulation )
* 외부에서 직접 접근하지 못하게 보호하는 개념 ( why? 정보은닉 )
* 접근제어자를 통해야만 변경이 가능하다.   
-> 접근제어자 : 클래스, 변수, 매서드, 생성자의 접근 범위를 제한하는 키워드이다.

### 접근제어자 종류 

| 접근 제어자 | 클래스 내부 | 패키지 내부 | 상속한 클래스 | 전체공개 |
|-------------|-------------|--------------|----------------|-----------|
| public      | O           | O            | O              | O         |
| protected   | O           | O            | O              | X         |
| default     | O           | O            | X              | X         |
| private     | O           | X            | X              | X         |

### 게터(Getter), 세터(Setter)

캡슐화가 잘 적용된 클래스는 내부 데이터를 private로 보호하고있다
데이터 조회나 변경이 필요한 경우 게터, 세터를 이용한다.

---

### 캡슐화, 접근제어자, 게터, 세터 예제

```java
// Main class

public class Main {
    public static void main(String[] args) {
        // 생성자 호춣
        Person person = new Person("Kim");

        // 인스턴스 변수 접근
//        person.name = "kim";
//        person.secret = "???";

        // 인스턴스 메서드 접근
//        person.methodA();
//        person.methodB();

        // 게터
        String name = person.getName();
        System.out.println("name = " + name);

        // 세터
        person.setName("Steve");
        String name2 = person.getName();
        System.out.println("name2 = " + name2);
    }
}

```

---

```java
// Person class

public class Person {
    // 속성
    private String name;
    private String secret;

    // 생성자 ( 기본은 default 이다 )
    public Person(String name) {
        this.name = name;
    }

    // 기능
    public void methodA(){}
    private void methodB(){}

    // 게터
    public String getName(){
        return name;
    }

    // 세터
    public void setName(String name){
        this.name = name;
    }
}
```

---

## 상속 ( extends )
* 클래스간의 관계를 부모, 자식으로 바라보는 개념   
* extends 를 사용하여 상속관계를 구현한다.   
* 재사용성, 확장이 가능하다.   

### super
* 부모 클래스의 맴버에 접근할 때 사용하는 키워드이다.   
*자식 클래스에서 부모의 변수나 매서드를 명확하게 호출할 때 사용한다.   

### super()
* 부모가 먼저 생성되고 자식이 생성되어야 한다.   
* 부모가 먼저 생성되어야 하므로 super()은 항상 생성자의 첫줄에 위치해야한다.   

### Overriding ( 오버라이딩 ) 
* 부모 메서드를 자식 클래스에서 변경하여 재정의하는 것을 의마한다.   
-> ( @Overriding 키워드를 붙인다 )
* 매서드 이름,타입,매개변수가 완전히 동일해야한다.    
* 접근 제어자는 부모보다 더 강한 수준으로만 변경 가능하다.   

---

### 상속, super, super(), Overriding 예제

```java
// Parent class

public class Parent {
    public String faliyName = "스파르탄";
    public  int honor = 10;

    public Parent(){
        System.out.println("부모 생성자");
    }

    public void introduceFamily(){
        System.out.println("우리 " + this.faliyName + "가문은 대대로...");
    }
}
```

---

```java
// Child class

public class Child extends Parent{
    public String familName = "Kim";

    public Child(){
        super();
        System.out.println("자식 생성자");
    }

    public void superTest(){
        System.out.println(super.faliyName);
    }

    public void showSocialMedia(){
        System.out.println("SNS에서 우리 가문을 소개드립니다");
    }

    @Override
    public void introduceFamily(){
        System.out.println("오버라이드");
    }
}
```

---

```java
// Main class

public class Main {
    public static void main(String[] args) {
        Child child = new Child();

        System.out.println("가문이름 : " + child.faliyName);
        System.out.println("명예 : " + child.honor);
        child.introduceFamily();
        child.superTest();
        child.showSocialMedia();
    }
}

// 결과값
// 부모 생성자
// 자식 생성자
// 가문이름 : 스파르탄
// 명예 : 10
// 오버라이드
// 스파르탄
// SNS에서 우리 가문을 소개드립니다
```
