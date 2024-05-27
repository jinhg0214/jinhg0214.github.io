---
title: "auto로 생성된 iterator는 무엇일까"
date: 2024-05-27 16:09:00 +0900
categories: [C++, keyword]  # 최대 2개 가능
tags: [iterator, auto, rangebasedfor, referenceoperator]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://i.ytimg.com/vi/2vOPEuiGXVo/maxresdefault.jpg"
    alt: 
---

## 상황

알고리즘 문제를 풀던 중, C++ unordered map 의 원소를 순회해야하는 경우가 생겼다

이를 위해 다음과 같이 코드를 작성했다

```c++
unordered_map<char, int> um;

for (char ch : str) {
	um[c]++;
}

for(auto iter : um){
	cout << um.first << " : " << um.second << '\n'; 
}
```
문자열에 들어있는 알파벳의 개수를 계산하는 과정이였다

이후 auto 키워드가 가시성이 떨어진다 생각하여 

`for(auto iter : um)`를

`for(unordered_map<char,int>::iter = um.begin(); iter != um.end(); iter++)` 로 수정했다

그런데 여기서 문제가 발생했다

`um.first`, `um.second`와 같이 객체의 멤버에 접근하는 연산자가 빨간줄을 띄웠다; 

뭐가 문제인가 싶어 `um->first`, `um->second` 연산자로 수정해보니 작동했다

이를 잘 이해하기 위해 한번 정리해보았다

## .(도트) 연산자와 ->(화살표) 연산자

- 둘다 모두 구조체에서 변수를 사용할 때 이용하는 연산자다 무언가에 접근한다는 의미로 사용됨
- 둘의 차이는 결정적으로, 포인터인지 아닌지에 따라 결정된다

### `.` 연산자
- `.` 연산자는 객체의 멤버에 <font color="red">직접</font> 접근할 때 사용한다
- 객체가 직접 접근 가능한 경우 사용한다. 즉, 일반적인 변수로 선언되어있을 때 사용함

### `->` 연산자

- `->` 연산자는 객체에 대한 포인터를 통해 멤버에 <font color="red">간접</font> 접근할 때 사용한다
- 포인터를 통해 객체의 멤버에 접근할 때, 먼저 포인터를 역참조한 다음, 멤버에 접근해야 하는데 `->` 연산자는 이 두 단계를 한번에 수행해준다 

## auto 키워드

`for(auto iter : um)`에서는 범위기반 for 루프에 auto 키워드를 이용하여 iterator를 선언하였다

auto 키워드는 선언된 변수의 초기화 식을 사용하여 해당 형식을 추론하여 컴파일러에 지시하여 자동으로 결정한다

즉 여기서 어떤 과정으로 자동으로 생성되었는지 자세히 살펴보면

1. auto iter 는 `um`의 각 원소를 복사함
2. `iter`는 `std::pair<const char, int>` 타입을 가짐
3. `iter`는 원소의 복사본이므로, `.`연산자를 사용하여 `first`와 `second`에 접근 가능

`for(unordered_map<char,int>::iter = um.begin(); iter != um.end(); iter++)` 에서는

1. 명시적으로, `unordered_map<char, int>::iterator` 타입으로 선언하였다
2. iterator는 `um`의 원소를 가리키는 포인터와 유사하다
3. `iter->first`는 `(*iter).first`와 동일함. 즉 이 단계를 ->로 한단계로 줄인것 뿐임

이런 차이가 있다

즉 auto는 원소에 복사본이므로 `.`연산자를 사용했고, 명시적 선언은 포인터기 때문에 `->`를 사용했다

## auto는 그럼 메모리 효율이 괜찮나?

auto iter는 배열의 원소를 복사하여 참조한다고 했다

그렇다면 원소 복사에 따른 메모리 사용이 있지 않을까?

검색해보니 범위기반 for 루프에서 `auto iter`를 사용할 때, 원소가 복사된다는 것은

각 반복에서 배열의 원소가 새로운 `std::pair<const char, int>` 객체로 만들어진다는 의미라고 한다

즉, 원소 타입이 엄청나게 크거나 복사 비용이 큰 경우를 제외하면 크게 메모리 성능에 영항을 주지 않는다는것

그러나 이런 영향도 고려하고 싶다면

`for(auto& iter : um)` 처럼 & 참조 연산자를 사용하여 원소를 참조할 수 있다. 

사용도 간편하고 메모리 사용과 성능 측면에서 더 효율적이므로, 이 방법을 사용하는게 좋아보인다


