---
layout: post
title: Optional
subtitle: Optional<>
categories: TIL
tags: [TIL]
---

## Optional 
* null을 안전하게 다루게 해주는 객체이다.   
-> null : 프로그램에서 값이 없음 또는 참조하지 않음을 나타내는 키워드   

* null을 직접 다루는 대신 Optional을 사용하면 NullPointerException을 방지할 수 있다.

### NPE(NullPointerException)
* 개발을 할 때 가장 많이 발생하는 예외 중 하나가 바로 NPE(NullPointerException)이다.    
* NPE를 피하려면 null 여부를 검사해야 하는데, null 검사를 해야하는 변수가 많은 경우 코드가 복잡해지고 번거롭다.    
그래서 null 대신 초기값을 사용하길 권장하기도 한다.   


### Optional 이 왜 필요할까? 
* Optional<T>는 null이 올 수 있는 값을 감싸는 Wrapper 클래스로, 참조하더라도 NPE가 발생하지 않도록 도와준다.    
* Optional 클래스는 아래와 같은 value에 값을 저장하기 때문에 값이 null이더라도 바로 NPE가 발생하지 않으며, 클래스이기 때문에 각종 메소드를 제공해준다.    
* null을 직접처리를 할수는 있지만, 모든코드에서 null이 발생할 가능성을 미리 알고 처리하는것은 현실적으로 불가능하기 때문에 Optional을 사용한다.

### Optional 예제

```java
// Student class
public class Student {
    // 속성
    private String name;

    // 생성자
    public Student(String name){
        this.name = name;
    }

    // 기능
    public String getName(){
        return name;
    }
}

```

---

```java
// Camp class
public class Camp {
    // 속성
    private Student student;

    // 생성자

    // 기능
    public Optional<Student> getStudent() {
        return Optional.ofNullable(student);
    }

    public void setStudent(Student student) {
        this.student = student;
    }
}

```

---

```java
    public static void main(String[] args) {
        Camp camp = new Camp();
        Student steve = new Student("Steve");
        camp.setStudent(steve);

        // Optional 객체 활용
        Optional<Student> studentOptional = camp.getStudent();
        boolean flag = studentOptional.isPresent();

        if(flag){
            Student student = studentOptional.get();
            String studentName = student.getName();
            System.out.println("studentName = " + studentName);
        } else System.out.println("학생 데이터가 없습니다.");
    }
    // 결과값 
    // studentName = Steve
```

---

## orElseGet() 
* orElseGet() 은 값이 없을 때만 기본 값을 제공하는 로직을 실행하는 메서드이다.
* 이것을 제대로 활용하려면 Lambda식 표현 방법을 사용할 줄 알아야 한다.

```java
Student student = camp.getStudent()
                       .orElseGet(() -> new Student("미등록 학생"));
```

저런식으로 활용하는것이 Lambda식 표현인데 이 표현은 다음에 알아볼 것이다.

---

## 느낀점
null값은 신경쓰지 않고 했었는데 Optional이라는 객체가 있다는걸 알게되어 앞으로는 신경써서 예외처리를 할 수 있게 되었다.    
