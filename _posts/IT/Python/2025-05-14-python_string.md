---
title: "파이썬 문자열 문법 정리"
date: 2025-05-14 17:09:55 +0900
categories: [Python, Grammar]  # 최대 2개 가능
tags: tags: [string, formatting, indexing, slicing, immutable, method]
toc: true
comment: false
published: false
image:
    path: "https://miro.medium.com/v2/resize:fit:656/1*Ib1xRU1vcHlhmzL5LdIrrg.jpeg"
    alt: 
---

## 1. 문자열 만들기

1. 큰 따옴표 혹은 작은 따옴표로 양쪽 둘러 싸기 : `"Hello World"`, `'Hello World`
2. 큰 따옴표 혹은 작은 따옴표를 연속으로 3개 써서 양쪽 둘러싸기 `"""Hello\nWorld"""`
	- 여러 줄의 문자열을 표현할 수 있음
	
### 1-1. 문자열 변수 이름

- 일반적으로 snake case를 사용함
	- `snake_case = "snake kase"`
- 특수문자는 `_`만 사용함
- 변수 이름 맨 앞에 숫자가 나와서는 안된다
- 문자열 변수명에 `str`은 피할 것

---

## 2. 문자열 연산

- `+` : 문자열 연결
- `*` : 문자열 반복

---

## 3. 문자열 길이 구하기

- `len(a)` : 파이썬 내장 함수. 길이 출력

---

## 4. 문자열 인덱싱과 슬라이싱

`>>> a = "Life is too short, You need Python"`

### 4-1. 인덱싱

- 파이썬도 0부터 시작함 
	- `a[3] = 'e'`
- 뒤에서부터 카운팅 가능 
	- `a[-2] = 'o'`

### 4-2. 슬라이싱

- `a[시작_번호 : 끝_번호]` 범위 지정하여 슬라이싱 가능. 시작번호는 포함, 끝번호는 포함하지 않음
	- `a[0:4] = "Life"` 
- `a[시작_번호 :]` 끝번호를 생략하면 시작 번호부터 끝까지 
	- `a[19:] = "You need Python"`
- 반대로 시작 번호를 생략하면 처음부터 끝번호까지
	- `a[:17] = "Life is too short`
- 슬라이싱도 마찬가지로 `-`기호 사용 가능
	- `a[19:-7] = 'You need'` 
- 날짜 나누기 예시

```python
>>> a = "20230331Rainy"
>>> date = a[:8]
>>> weather = a[8:]
>>> date
'20230331'
>>> weather
'Rainy'
```

- **문자열은 Immutable 자료형임**

```python
>>> a = "Pithon"
>>> a[1]
'i'
>>> a[1] = 'y'
TypeError: 'str' object does not support item assignment
할당 불가 에러 발생함
```

다음과 같이 문자열을 바꾸려면 `a[:1] + 'y' + a[:2]` 로 직접 만들어줘야함

---

## 5. 문자열 포매팅

### 5-1. 직접 대입

- `%d` : 숫자 대입. `%` 기호 뒤에 숫자를 넣어줘야함
	- `"I eat %d apples." % 3`
- `%s` : 문자열 대입
	- `"I eat %s apples." % "five"`
	- 문자열 포맷 코드는 자동으로 정수, 소수 등을 문자열로 형 변환을 하여 대입함
- 변수 대입
	-  `"I eat %d apples." % number`
- 2개 이상 대입
	- `"I ate %d apples. so I was sick for %s days." % (number, day)`

|코드|설명|
|---|---|
|%s|문자열(String)|
|%c|문자 1개(character)|
|%d|정수(Integer)|
|%f|부동소수(floating-point)|
|%o|8진수|
|%x|16진수|
|%%|Literal % (문자 `%` 자체)|

### 5-2. 정렬과 공백, 소수점

- `"%10s" % "hi"` : `        hi` : 문자열이 10개인 공간에서 오른쪽으로 정렬하고 나머지는 고앱ㄱ으로
- `"%-10sjane." % 'hi` : 'hi        jane' : hi는 왼쪽 정렬, 나머지는 공백으로 채운 후 jane 출력

### 5-3. format 함수를 사용한 포매팅

- `"I eat {0} apples".format(3)` : 문자열의 `.format()` 함수를 호출하여 `{0}`에 3을 넣음
- `"I ate {0} apples. so I was sick for {1} days.".format(number, day)` 
- `"I ate {number} apples. so I was sick for {day} days.".format(number=10, day=3)`

### 5-4. 정렬

- `"{0:<10}".format("hi")` : `:<10` 왼쪽 정렬 후, 10자리로 맞춤
- `"{0:>10}".format("hi")` : `:>10` 오른쪽 정렬 후, 10자리로 맞춤
- `"{0:^10}".format("hi")` : `^10` 가운데 정렬
- `"{0:=^10}".format("hi")` : 공백을 =로 채우기

### 5-5. f 문자열 포매팅

- 파이썬 3.6 버전부터 지원하는 기능
- `f'나의 이름은 {name}입니다. 나이는 {age}입니다.'` 로 사용하면 변수값을 참조 가능
- `f'나는 내년이면 {age + 1}살이 된다.'` 와 같이 계산식도 사용 가능
- `f'나의 이름은 {d["name"]}입니다. 나이는 {d["age"]}입니다.'` 딕셔너리형도 사용 가능

---

## 6. 문자열 관련 함수들

- `count` : 특정 문자 개수 세기
	- `a.count('b') = 2`
- `find` : 특정 문자 위치 검색. 첫번째 인덱스를 반환. 검색 결과가 없으면 `-1`을 반환
	- `a = "Python is the best choice"`, `a.find('b') = 14`, `a.find('k') = -1`
- `index` : find와 유사한 동작. 그러나 검색 결과가 없으면 오류가 발생
- `join` : 문자열 삽입
	- `",".join('abcd') = 'a,b,c,d`
- `upper, lower` : 대소문자 전환
- `lstrip`, `rstrip`, `strip` : 좌우 공백 제거
- `replace(바뀔_문자열, 바꿀_문자열)` : 문자열 바꾸기 및 제거
	- `print(a.replace('a', ''))` : a를 찾아 지우는 효과
	- `print(a.replace(' ', ''))` : 공백 지우기
	- `print(a.replace('', 'z'))` : 모든 글자 사이에 z를 추가하기
- `split` : 문자열 나누기. 매개변수가 없다면 공백(space, tab, enter)기준으로 나눠 리스트에 넣음
	- `list(문자열)` : 각 문자를 하나씩 리스트에 추가함
	- 반대로 리스트를 문자열로 바꾸고 싶다면 `"{구분자}".join(list)`를 사용함
- `isalpha` : 해당 문자열이 알파벳으로만 구성되어 있는지 체크
- `isdigit` : 숫자로만
- `startswith` : 특정 문자열로 시작하는지 체크
- `endswith` : 특정 문자열로 끝나는지



