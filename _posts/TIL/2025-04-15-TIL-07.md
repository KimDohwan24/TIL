---
layout: post
title: 객체지향 이해하기 - 3
subtitle: abstract, Polymorphism
categories: TIL
tags: [TIL]
---

## 추상클래스 ( abstract )
* 공통 기능을 제공하면서 하위 클래스에 특정 메서드를 구현을 강제하기 위해 사용한다.   
* 추상 클래스는 인스턴스화 할 수 없다.   

## 추상 클래스와 인터페이스의 차이점
* 계층적 구조를 표현하면서 공통 속성과 기능을 재사용할때 추상 클래스를 사용하는것이 적합하다. 

## 추상화와 다형성
* 추상화 : 특정 계층에서 불필요한 정보를 제거하고 본질적인 특징만 남기는것   
* 다형성 : 하나의 타입으로 여러 객체를 다룰 수 있는 특징   
  
## 형변환 
* 부모타입으로 자식타입을 다룰 수 있는 이유는 자동으로 형변환(Config)이 발생했기 때문이다.
* [ 부모 -> 자식 ] : 업케스팅   
* [ 자식 -> 부모 ] : 다운캐스팅   

### 업캐스팅 주의사항
* 자식타입의 고유 기능을 사용할 수 없다. -> 사용하려면 다운캐스팅이 필요하다.   

### abstract 예제 
```java
// LifeForm class

public interface LifeForm {
    void exist(); // 공통 : 모든 생명체는 존재한다.
}
```

---

```java
// Animal class 

public interface Animal extends LifeForm {
    void makeSound(); // 공통 : 모든 동물은 소리를 냅니다.
}
```

---

```java
// Cat class

public class Cat implements Animal{
    @Override
    public void makeSound() {
        System.out.println("고양이가 존재합니다");
    }

    @Override
    public void exist() {
        System.out.println("야옹");
    }

    public void scratch(){
        System.out.println("스크래치");
    }
}
```

---

```java
// Main class

public class Main {
    public static void main(String[] args) {
        Cat cat = new Cat();
        cat.exist();
        cat.makeSound();
        cat.scratch();
    }
}
// 결과값

// 야옹
// 고양이가 존재합니다
// 스크래치
```


### 다형성( Polymorphism ) 예제 
```java 
// LifeForm class

public interface LifeForm {
    public void exist();
}
```

---

```java
//  Animal class
public interface Animal extends LifeForm{
    void makeSound();
}
```

---

```java
// Cat class

public class Cat implements Animal{
    @Override
    public void exist() {
        System.out.println("고양이가 존재합니다");
    }

    @Override
    public void makeSound() {
        System.out.println("야옹");
    }

    public void scratch(){
        System.out.println("스크래치");
    }
}
```

---

```java
// Dog class

public class Dog implements Animal{
    @Override
    public void exist() {
        System.out.println("강아지가 존재합니다");
    }

    @Override
    public void makeSound() {
        System.out.println("멍멍");
    }
    public void wag(){
        System.out.println("흔들흔들");
    }
}
```

---

```java
// Main class

public class Main {
    public static void main(String[] args) {
        Animal animal1 = new Cat();
        Animal animal2 = new Dog();

        animal1.exist();
        animal1.makeSound();

        animal2.exist();
        animal2.makeSound();

        // 업캐스팅의 주의사항
//        animal1.scratch();
//        animal2.wag();

        // 다운 캐스팅
        Cat cat = (Cat) animal1;
        cat.scratch();

        Dog dog = (Dog) animal2;
        dog.wag();

        //  잘못된 다운캐스팅 문제
//        Cat cat2 = (Cat) animal2; // animal2 = Dog;
//        cat2.scratch();

        // 다운캐스팅 instanceof 활용 방법
        if(animal2 instanceof Cat){
            Cat cat2 = (Cat) animal2;
            cat2.scratch();
        } else System.out.println("객체가 고양이가 아닙니다!");

        Animal[] animals = {new Cat(), new Dog(), new Cat()};
        System.out.println(":::");
        for(Animal animal : animals){
            animal.makeSound();
        }
    }
}

// 결과값 
// 고양이가 존재합니다
// 야옹
// 강아지가 존재합니다
// 멍멍
// 스크래치
// 흔들흔들
// 객체가 고양이가 아닙니다!
// :::
// 야옹
// 멍멍
// 야옹
```
