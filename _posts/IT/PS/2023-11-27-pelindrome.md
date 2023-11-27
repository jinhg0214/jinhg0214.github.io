---
title: "팰린드롬 판별법"
date: 2023-11-27 21:19:00 +0900
# last_modified_at: 
categories: [Algorithm, Problem Solving]  # 최대 2개 가능
tags: [string, pelindrome]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
---

[https://jinhg0214.github.io/posts/bj_1990/](https://jinhg0214.github.io/posts/bj_1990/)를 복귀하던 중

소수 판별을 개선하는 방법도 있는데

팰린드롬도 더 효율적으로 찾는 방법이 있지 않을까 생각이 들어서 

찾아서 정리해봄

# 팰린드롬이란?
---
회문(Palindrome)

거꾸로 읽어도 제대로 읽는 것과 같은 문장이나 낱말, 숫자, 문자열

수박이박수, racecar, 1234321, 다시합창합시다


# 1. 기본 방법
---
- 첫 문자와 끝 문자부터 시작하여 안쪽으로 차례차례 비교한다
- 가장 직관적인 방법
- 펠린드롬이 맞는 경우, N/2 번 반복문이 실행되므로 시간 복잡도는 O(N)
```cpp
bool IsPalindrome(string s){
    int size = s.size();

    for(int i=0; i < size/2; i++){
        if(s[i] != s[size-1-i]{
            return false;
        })
    }

    return true;
}
```

# 2. STL을 이용하는 방법
---
- `reverse` 함수를 이용하는 방법
- 뒤집어진 문자열을 복사해 원문과 같은지 확인함
```cpp
bool IsPalindrome(string str){
    string str_rev = str;
    reverse(str_rev.begin(), str_rev.end()); 

    return str_rev == str;
}
```

- `equal` 을 이용하는 방법
- 원문의 시작지점 `str.begin()`부터 
- 원문의 중앙 지점 `str.begin() + str.size()/2` 까지
- 원문의 맨 뒤에서부터 비교하여 값을 확인한다
- 매우 짧게 가능
```cpp
bool IsPalindrome(string str){
    return equal(str.begin(), str.begin() + str.size()/2, str.rbegin()); 
}
```
- 이게 가장 간단해서 이걸 쓰는 연습을 해야할듯함

# DP(재귀)를 이용한 방법
---
- 함수에서 첫글자와 마지막 글자만 비교하고, 나머지 가운데는 재귀로 호출하는 방법
- `12321`을 받았다면, 시작지점의 `1`과 마지막 `1`을 비교하고 맞다면 `232`에 대한 펠린드롬 체크를 호출함
```cpp
bool IsPalindrome(string str, int start, int end){
    if( start >= end ){ // 기저 조건 : 펠린드롬 판별이 끝난 경우
        return true;
    }

    // 글자를 비교
    if(str[start] != str[end]){
        return false;
    }

    // 부분 문자열에 대해 펠린드린 체크 호출
    return IsPalindrome(str,start+1, end-1);
}
```

# 숏코딩
```cpp
#import<bits/stdc++.h>
std::string a;main(){std::cin>>a;std::cout<<equal(a.begin(),a.end(),a.rbegin());}
```
```python
a=input();print(+(a[::-1]==a))
```
이게 무슨