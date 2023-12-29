---
title: "C++에서 대소문자 변환 방법"
date: 2023-12-29 18:56:00 +0900
categories: [C++, Library]  # 최대 2개 가능
tags: [tolower, toupper, convertcase, boost, algorhtim]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
---

알파벳 대소문자 변환 방법들 모음

1. ASCII를 이용한 직접 변환
2. `std::transform` 사용
3. `std::toupper` 와 `std::tolower` 사용
4. `boost::to_upper` 와 `to_lower`사용

# 1. ASCII를 이용한 직접 변환
---
- A는 65, Z는 90, a는 97, z는 122를 이용함
- 소문자에서 대문자 전환은 32를 빼주고, 대문자에서 소문자 전환은 32를 더해준다
```cpp
// 소문자로 변환하는 함수
string my_to_lower(string str) {
	string result;
	for (char ch : str) {
		if ('A' <= ch && ch <= 'Z') {
			result.push_back(ch + 32);
		}
		else {
			result.push_back(ch);
		}
	}
	return result;
}
```

# 2. std::transform 을 이용한 변환
---
- [[algorithm 라이브러리]]에 포함된 transform 함수를 이용한다

```cpp
template< class InputIt, class OutputIt, class UnaryOperation > 
OutputIt transform( InputIt first1, InputIt last1, OutputIt d_first, UnaryOperation unary_op );
```
- 로 정의되어있음
- input을 사용자가 정의한 원하는 방법에 따라 output으로 바꿔주는 함수
- 간단히 설명하면
```cpp
Output transform(first 시작점, first 끝점, 저장할 변수의 시작점, 변환할 함수)
```

대소문자 전환은 
`transform(str.begin(), str.end(), str2.begin(), ::toupper);` 과 같이 사용하면됨

```cpp
void lowercase(string s) { 
	transform(s.begin(), s.end(), s.begin(), ::tolower); 
	// string s가 HELLO라면 tranform을 수행한 후 hello로 변경. 
}
```
과 같이 python 처럼 구현가능

# 3. `std::toupper` 와 `std::tolower` 사용
--- 
- 한글자만 변경 가능함. 즉, char 하나만 가능

```cpp
for(int i=0; i<str.size(); i++){
	str[i] = toupper(str[i]);
}
```
처럼 사용해야 변환 가능함

# 4. `boost::to_upper` 와 `to_lower`사용
---
- [[Boost 라이브러리]]에 속해있는 변환 함수
- transform 보다 간단하게 `boost::to_upper(str)` 로 사용하면, str에 저장됨
- 혹은 원본은 손상시키고 싶지 않다면 `string ret = boost:to_upper_copy(str)` 처럼 사용 가능하다