---
title: "cin과 getline을 같이 쓰는 방법 : cin.ignore"
date: 2024-02-15 19:10:00 +0900
categories: [C++, io]  # 최대 2개 가능
tags: [input, output, cin, getline, ignore]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
---

# 상황
---

알고리즘 문제를 풀던 중,

테스트케이스 N과

띄어쓰기를 포함한 문장 N개를 입력받아야하는 상황이 나왔다

```cpp
int N;
cin >> N;
while(T--){
	string input; cin >> input;
	cout << input;
}
```

대충 이런식으로 작성했는데

첫번째 케이스에서 input이 `""` 으로 공백만 입력되는 상황이 발생했다

# 원인
---

> C++ 프로그램은 파일이나 콘솔의 입출력을 직접 다루지 않고,    
> 스트림(stream)이라는 흐름을 통해 다룹니다   
> 스트림(stream)이란 실제의 입력이나 출력이 표현된 데이터의 이상화된 흐름을 의미합니다   
> 즉, 스트림은 운영체제에 의해 생성되는 가상의 연결 고리를 의미하며, 중간 매개자 역할을 합니다

C++ 프로그램은 파일이나 콘솔의 입출력을 직접 다루지 않고, 스트림이라는 흐름을 통해 다룸

스트림 내부에 버퍼라는 임시 메모리 공간을 가지고 있다.

> 버퍼를 사용하면 다음과 같은 장점이 있습니다.   
> 문자를 키보드 하나 누를때마다 바로바로 전달하는것이 아니라,       
> 한번에 묶어서 전달하므로, 전송시간이 적게 걸려 성능이 향상됩니다.  
> 또한, 사용자가 문자를 잘못 입력한 경우 수정할 수 있습니다

즉, C++은 스트림을 통해서만 입출력을 다루고, 그 안에 버퍼가 있는데

`cin >> val;` 형태로 사용하면, 입력 버퍼 상에 `\n` 문자는 읽지 않기 때문에 버퍼에 `\n`이 남게 되는것

이후 `getline(cin, str)`을 수행하면, 남아있던 `\n` 문자를 getline이 읽어, 빈 문자열만 읽는 것이 원인이였다


# 해결 방법
---

`cin.ignore()`라는 버퍼의 글자를 일부 무시하는 함수를 사용하여 해결한다

Java의 flush처럼 버퍼를 비우는 것이 아니라, 버퍼의 앞 몇글자를 무시하고 입력받는것임

### 예시

```cpp
int n;
string s;

cin >> n;
cin.ignore(4);

getline(cin, s);
cout << s << endl;
```
입력 :
3
a
bcd
출력 : cd

n에 3을 저장한뒤의 입력 버퍼 상황을 보면

`[\n][a][\n][bcd\n]` 이 남아있음 ([]는 편의상 넣음)

`cin.ignore(4)`에 의해 `getline(cin, s);` 수행시, `[\n][a][\n][b]`는 무시되고 

`cd\n`만 s에 담기게 된다

버퍼를 비우는것이 아니라 무시하고 담는것이므로 사용에 주의한다

---

참조 :

[스트림과 버퍼 - TCP School](https://tcpschool.com/cpp/cpp_io_streamBuffer)

[# cin과 getline을 같이 사용할때 cin.ignore()이 필요한 이유 기록 - namwhis](https://namwhis.tistory.com/entry/cin%EA%B3%BC-getline%EC%9D%84-%EA%B0%99%EC%9D%B4-%EC%82%AC%EC%9A%A9%ED%95%A0%EB%95%8C-cinignore%EC%9D%B4-%ED%95%84%EC%9A%94%ED%95%9C-%EC%9D%B4%EC%9C%A0-%EA%B8%B0%EB%A1%9D)

[C++ getline 사용시 buffer clear 문제 깔끔하게 해결하기 - 찬찬](https://blog.naver.com/cksdn788/221239805238)