---
layout: post
title: Validation, BindingResult
subtitle: 검증(validation), BindingResult
categories: TIL
tags: [TIL]
---

## Validation(검증)
- 클라이언트에서 서버로 값을 전달하고자 할 때 (`@RequestBody`, `@RequestParam`, `@PathVariable`) 전달되는 데이터에 대해 유효성 검증을 수행하며 유효하지 않을 경우 에러

```java
@PostMapping("/users")
public ResponseEntity<?> createUser(@Valid @RequestBody UserDTO userDTO) {
    // 유효성 검사 통과 시 실행됨
    return ResponseEntity.ok("Created");
}
```

### Validation의 주요 기능
- `@Valid` 또는 `@Validated` 어노테이션을 사용하여 Spring이 요청 데이터를 자동으로 검사하게 할 수 있다.
- 유효성 실팻히 `MethodArgumentNotValidException`이 발생하여 자동으로 400에러 응답을 줄 수 있다.

```java
@PostMapping("/users")
public ResponseEntity<?> createUser(@Valid @RequestBody UserDTO userDTO) {
    // 유효성 통과 후 로직 실행
    return ResponseEntity.ok().build();
}
```

### Validation 어노테이션 

| 카테고리 | 주요 어노테이션                                   |
| ---- | ------------------------------------------ |
| 문자열  | `@NotBlank`, `@Size`, `@Email`, `@Pattern` |
| 숫자   | `@Min`, `@Max`, `@Positive`, `@Digits`     |
| 날짜   | `@Past`, `@Future`, `@PastOrPresent`       |
| 공통   | `@NotNull`, `@NotEmpty`, `@Null`           |

#### 문자열 관련 어노테이션

| 어노테이션              | 설명                                                |
| ------------------ | ------------------------------------------------- |
| `@NotBlank`        | 공백이 아닌 문자열이어야 함 (null, "", "  " 모두 허용 안 됨)        |
| `@NotEmpty`        | null이 아니고 길이가 0 이상이어야 함 (`""` 허용 안 됨, `" "`은 허용됨) |
| `@Size(min, max)`  | 문자열 또는 컬렉션의 길이가 min 이상, max 이하                    |
| `@Pattern(regexp)` | 정규표현식에 부합해야 함                                     |
| `@Email`           | 이메일 형식이어야 함 (기본적으로 RFC 5322 준수)                   |

#### 숫자 관련 어노테이션

| 어노테이션                        | 설명              |
| ---------------------------- | --------------- |
| `@Min(value)`                | 지정된 값 이상        |
| `@Max(value)`                | 지정된 값 이하        |
| `@Positive`                  | 0보다 커야 함        |
| `@PositiveOrZero`            | 0 또는 양수         |
| `@Negative`                  | 0보다 작아야 함       |
| `@NegativeOrZero`            | 0 또는 음수         |
| `@Digits(integer, fraction)` | 정수부와 소수부 자릿수 제한 |

#### Null 관련 어노테이션

| 어노테이션      | 설명                            |
| ---------- | ----------------------------- |
| `@NotNull` | null이 아니어야 함 (빈 문자열 `""`은 허용) |
| `@Null`    | 반드시 null이어야 함                 |

#### 날짜 / 시간 관련 어노테이션

| 어노테이션              | 설명          |
| ------------------ | ----------- |
| `@Past`            | 현재보다 과거 날짜  |
| `@PastOrPresent`   | 과거 또는 오늘 날짜 |
| `@Future`          | 현재보다 미래 날짜  |
| `@FutureOrPresent` | 미래 또는 오늘 날짜 |

#### 컬렉션 관련 어노테이션 

- `@NotEmpty` : 컬렉션, 배열, 맵 등에 적용 가능 (null 또는 빈 경우 유효하지 않음)
- `@Size` : 리스트/배열 등의 길이 제한

### BindingResult
- `@BindingResult`는 유효성 검사 결과를 담은 객체이다.
- `@Valid` 나 `@Validated` 뒤에 바로 파라미터로 선언하면, 유효성 검사에서 오류가 발생해도 예외가 발생하지 않고 에러 정보를 이 객체를 통해 `직접 처리할 수 있다`.

```java
@PostMapping("/users")
public ResponseEntity<?> createUser(@Valid @RequestBody UserDTO userDTO,
                                    BindingResult bindingResult) {
    if (bindingResult.hasErrors()) {
        // 유효성 오류 처리
        return ResponseEntity.badRequest().body(bindingResult.getAllErrors());
    }

    // 유효성 검사 통과
    return ResponseEntity.ok("Created");
}
```

- 중요한 규칙 : `@Valid` 또는 `@Validated` 뒤에 바로 위치한 파라미터여야 유효성 검사 결과가 BindingResult에 담깁니다.

### Valid vs Validated 차이점

| 항목    | `@Valid`                     | `@Validated`     |
| ----- | ---------------------------- | ---------------- |
| 소속    | `javax.validation` (JSR-303) | Spring Framework |
| 그룹 지정 | ❌ 안 됨                        | ✅ 가능 (`groups`)  |
| 실무 사용 | 일반적인 DTO 검사                  | 그룹별 조건 검사 시 사용   |

## 요약

| 항목                      | 설명                                             |
| ----------------------- | ---------------------------------------------- |
| `@Valid` / `@Validated` | DTO에 선언된 Bean Validation 애너테이션을 기준으로 유효성 검사 수행 |
| `BindingResult`         | 유효성 검사 결과(에러 포함)를 담고 있음. 예외를 막고 직접 오류 처리 가능    |
| 사용 순서                   | 항상 `@Valid` 또는 `@Validated` **바로 뒤에 위치해야 함**   |
