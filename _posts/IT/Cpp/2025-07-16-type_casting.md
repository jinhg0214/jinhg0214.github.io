---
title: "C++ 형 변환 4종 비교"
date: 2025-07-16 10:44:00 +0900
categories: [C++, typecast]  
tags: [static-cast, dynamic-cast, c-style-cast]     
toc: true
comment: false
published: true
image:
    path: "https://www.softwaretestinghelp.com/wp-content/qa/uploads/2019/06/Type-Conversions-in-C.png"
    alt: 
---

## 형 변환(Type Casting)이란?

특정 데이터 타입의 값을, 다른 데이터 타입의 값으로 변환하는 과정

정수(integer)를 실수(flaot)로 바꾸거나, 부모 클래스 포인터를 자식 클래스 포인터로 바꾸는 등

형 변환은 크게 두가지로 나뉜다

1. 암시적 형 변환(Implicit Type Conversion) 
	- 컴파일러가 특정 상황에서 자동으로 형 변환을 수행
	- 예를들어 정수와 실수를 더할 때, 정수가 자동으로 실수로 변환됨
2. 명시적 형 변환(Explicit Type Conversion)
	- 프로그래머가 코드상에 직접 형 변환을 지시함
	- C++에서는 4가지 연산자를 제공함

---

## C 스타일 형 변환의 문제점

- `(new_type)expression`
- 가장 간단하지만, **안정성**, **명확성**, **코드 가독성** 때문에 지양해야하는 방식
1. 안정성 
	- 컴파일 타임에 충분한 타입 체크를 수행하지 않음
	- 서로 관련 없는 클래스 포인터끼리 변환하거나, 포인터를 정수로 바꾸는 등의 위험한 동작을 경고 없이 수행해버림
2. 명확성
	- C 스타일 캐스트는 `static_cast`, `const_cast`, `reinterpret_cast` 등 모든 변환을 시도해봄
	- 개발자의 의도가 static_cast였더라도, 실수로 reinterpret_cast가 필요한 상황을 만들면, 컴파일러가 오류 없이 이를 허용하는 경우가 발생할 수 있음
3. 코드 가독성
	- 어떤 종류의 변환이 일어났는지 명확하지 않아 코드의 의도를 파악하기 어려워짐
	- 또한, 코드 전체에서 형 변환이 일어나는 부분을 찾기 어려워짐. `(int)`같은 패턴은 일반적이기 때문!

---

## C++ 에서 지원하는 형 변환의 종류

C++은 C언어 스타일의 형 변환과 C++ 고유의 4가지 형 변환 연산자를 모두 지원한다

C스타일 보다는 C++스타일 연산자를 사용하는 것이 훨씬 안전하고, 의도가 명확하여 권장됨
### 1. `static_cast`
- 가장 일반적인 형 변환 연산자, 컴파일 시간에 논리적으로 변환 가능한 타입을 바꿀 때 사용함
- int, float, double 등 기본 숫자 타입 간의 변환
- void* 포인터를 다른 타입의 포인터로 변환
- 상속 관계이 있는 클래스 포인터/참조 간의 변환 (안전성이 보장된 업캐스팅 및 다운캐스팅)

```cpp
double d = 3.14;
int i = static_cast<int>(d); // i = 3

// 클래스 상속 관계에서의 사용
class Base{};
class Derived : public Base{};

Derived* d_ptr = new Derived();
Base* b_ptr = static_cast<Base*>(d_ptr); // 업캐스팅(안전)
```

### 2. dynamic_cast
- 다형성(Polymorphism)을 활용하는 클래스 계층 구조에서, 안전한 다운 캐스팅(down-casting)을 위해 사용함
- 런타임에 타입 정보를 확인하여 변환을 수행하며, 변환이 불가능할 경우 포인터는 `nullptr`를, 참조는 `std::bad_cast` 예외를 발생시킴
- 사용 시 기반 클래스에 반드시 하나 이상의 가상 함수(virtaul function)이 있어야함

```cpp
class Base{
	public:
	virtual void dummy() {} // 가상 함수
}
class Derived : public Base {};

Base* b_ptr = new Derived();

// b_ptr이 실제로 Derived 객체를 가리키는지 확인 후 다운 캐스팅
Derived* d_ptr = dynamic_cast<Derived*>(b_ptr);

if(d_ptr){
	cout << "형 변환 성공";
} else{
	cout << " 형 변환 실패";
}
```

### 3. const_cast

- 객체의 `const` 또는 `volatile` 속성을 제거할 때 사용하는 매우 제한적인 용도의 연산자
- `const`가 아닌 객체를 `const` 포인터/참조로 가리키고 있을 때, 원래의 `const`가 아닌 속성으로 되돌리기 위해 주로 사용함
- 원래 `const`로 선언된 객체의 `const`속성을 제거하고 값을 수정하려 하면 미정의 동작(Undefined Behavior)이 발생하므로 절대 사용해서는 안됨!!!

```cpp

void print(char *str){/*...*/};

const char* my_str = "hello";

// print 함수는 const char*가 아닌 char*를 인자로 받음
// my_str이 가리키는 내용이 변경되지 않음을 확신할 때만 사용함
printf(const_cast<char*>(my_str));
```

### 4. reinterpret_cast
- 가장 위혐한 형 변환으로, 한 타입의 비트(bit) 패턴을 전혀 다른 타입의 비트 패턴으로 재해석하라고 컴파일러에게 지시함
- 포인터와 정수 사이의 변환 등 로우레벨의 작업에 사용함
- 이 연산의 결과는 시스템/플랫폼에 따라 달라질 수 있어 이식성이 매우 떨어짐
- 꼭 필요한 경우 아니면 사용하지 말 것

```cpp
int i = 65
int* p_i = &i;

// int 포인터를 char 포인터로 재해석
char* p_c = reinterpret_cast<char*>(p_i);
// *p_c는 'A' (ASCII 65)를 가리킬 수 있음, 시스템에 따라 다름 

// 포인터 주소 자체를 정수로 변환
uintptr_t addr = reinterpret_cast<uintptr_t)(p_i);
```

---

## 정리

| 연산자              | 주요 용도                 | 특징            | 안전성    |
| ---------------- | --------------------- | ------------- | ------ |
| static_cast      | 논리적으로 연관된 타입 간 변환     | 컴파일 시간 검사     | 비교적 안전 |
| dynamic_cast     | 다형적 클래스의 안전한 다운캐스팅    | 런타임 검사        | 안전     |
| const_cast       | `const/volatile`속성 제거 | 타입 자체는 바꾸지 않음 | 위험     |
| reinterpret_cast | 관련 없는 타입 간의 비트 수준 변환  | 매우 낮은 수준의 변환  | 매우 위험  |

---

## 결론

C++프로그래밍 시에는 다음과 같은 원칙을 따른 것이 좋다

1. C 스타일 캐스트 `(type)expr`는 사용을 지양한다
2. 목적에 맞는 C++ 스타일 캐스트 연산자를 사용하도록 한다
	- 일반적 변환은 `static_cast`를 사용
	- 클래스 계층 구조에서 다운 캐스팅이 필요하면 `dynamic_cast`를 사용
	- `const_cast`와 `reinterpret_cast`는 정말로 필요하고, 그 위험성을 완전히 이해했을 때만 제한적으로 사용한다.