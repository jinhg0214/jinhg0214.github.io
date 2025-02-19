---
title: "bits/stdc++.h"
date: 2024-06-01 12:00:00 +0900
categories: [C++, Library]  # 최대 2개 가능
tags: [ps]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://matrixread.com/wp-content/uploads/2020/10/include-_bits_stdc.h_.png"
    alt: 
---

## 개요

알고리즘 문제를 풀다가 

내 코드는 iosteam, vector, queue, string 등등 include 할게 엄청 많아서 지저분한데

고수들의 코드를 보면 `#include <bits/stdc++.h>` 라는 한줄만 깔끔하게 있는 경우가 있다

대체 이 라이브러리는 뭐길래 이것만 쓰면 되는걸까 궁금해서 찾아보았다

## <bits/stdc++.h>란?
---
- Pre-compiled header의 일종으로, 사람들이 많이 사용하는 표준 라이브러리 헤더들을 모두 한번에 컴파일 될 수 있도록 모아놓은 라이브러리
- 즉, 자주 쓸만한 헤더 파일을 몽땅 include해놓은 헤더 파일
- gcc 계열의 컴파일러에서는 미리 내장되어 있다
	- Online Judge 사이트에서는 Linux 계열이기 때문에 `bits/stdc++.h` 사용 가능
- 그러나, Windows 계열에서는 Visual Studio/code를 사용. GNU 컴파일러나 GCC가 아닌 마이크로소프트가 자체적으로 개발한 Microsoft C/C++ Compiler(MSVC)를 사용함
	- 윈도우에서 사용하고 싶으면 헤더 파일을 수동으로 include 폴더에 넣어주어야 함

## 사용 이유?
---
- 알고리즘 대회같은 상황에서 `#include <라이브러리>`를 일일히 타이핑하는 시간을 줄일 수 있다
- 특정 함수를 위한 헤더를 찾는 시간을 줄일 수 있다

## 단점
---
- GNU C++의 표준 라이브러리 헤더가 아님. gcc 컴파일러에서 사용하는 헤더이므로 별도의 설정 필요
	- 이를 설정하는데 불필요한 작업과 시간 소요
- 소프트웨어 공학적으로는, 모든 헤더를 컴파일해야하는 상황이 발생하기 때문에, 불필요한 연산 작업이 들어가 컴파일이 느려지고 프로그램의 크기가 불필요하게 커짐
- 어떤 라이브러리의 어떤 함수를 사용했는지 추적하기 어려움
- 실제 프로젝트에서나 이식성을 고려해야하는 상황에서는 사용을 피하고, 필요한 헤더만 포함하는걸 권장함
  
## 설치 방법
---
- 리눅스 기반 gcc는 내장되어있음
- 윈도우 visual studio는 별도의 설치 필요
	- 현재 페이지 최하단 아래 내용을 복사해서 `stdc++.h` 파일 만들기
    - 혹은 직접 구해도 상관없음
	- 비쥬얼 스튜디오 경로의 `\Community\VC\Tools\MSVC\_사용 버전_\include`폴더에 
	- `bits` 폴더 생성 후, 여기다 넣기

## 참조
---
- [[Algorithm] <bits/stdc++.h>란?](https://cru6548.tistory.com/1)
- [# Visual Studio에서 <bits/stdc++.h> 사용하기](https://miniolife.tistory.com/11)


bits/stdc++.h 파일 

실제 내부 구현을 보면 PS에 자주 사용하는 라이브러리들을 몽땅 include했다

```
// C++ includes used for precompiling -*- C++ -*-

// Copyright (C) 2003-2013 Free Software Foundation, Inc.
//
// This file is part of the GNU ISO C++ Library.  This library is free
// software; you can redistribute it and/or modify it under the
// terms of the GNU General Public License as published by the
// Free Software Foundation; either version 3, or (at your option)
// any later version.

// This library is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.

// Under Section 7 of GPL version 3, you are granted additional
// permissions described in the GCC Runtime Library Exception, version
// 3.1, as published by the Free Software Foundation.

// You should have received a copy of the GNU General Public License and
// a copy of the GCC Runtime Library Exception along with this program;
// see the files COPYING3 and COPYING.RUNTIME respectively.  If not, see
// <http://www.gnu.org/licenses/>.

/** @file stdc++.h
 *  This is an implementation file for a precompiled header.
 */

// 17.4.1.2 Headers

// C
#ifndef _GLIBCXX_NO_ASSERT
#include <cassert>
#endif
#include <cctype>
#include <cerrno>
#include <cfloat>
#include <ciso646>
#include <climits>
#include <clocale>
#include <cmath>
#include <csetjmp>
#include <csignal>
#include <cstdarg>
#include <cstddef>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <ctime>

#if __cplusplus >= 201103L
#include <ccomplex>
#include <cfenv>
#include <cinttypes>
#include <cstdalign>
#include <cstdbool>
#include <cstdint>
#include <ctgmath>
#include <cwchar>
#include <cwctype>
#endif

// C++
#include <algorithm>
#include <bitset>
#include <complex>
#include <deque>
#include <exception>
#include <fstream>
#include <functional>
#include <iomanip>
#include <ios>
#include <iosfwd>
#include <iostream>
#include <istream>
#include <iterator>
#include <limits>
#include <list>
#include <locale>
#include <map>
#include <memory>
#include <new>
#include <numeric>
#include <ostream>
#include <queue>
#include <set>
#include <sstream>
#include <stack>
#include <stdexcept>
#include <streambuf>
#include <string>
#include <typeinfo>
#include <utility>
#include <valarray>
#include <vector>

#if __cplusplus >= 201103L
#include <array>
#include <atomic>
#include <chrono>
#include <condition_variable>
#include <forward_list>
#include <future>
#include <initializer_list>
#include <mutex>
#include <random>
#include <ratio>
#include <regex>
#include <scoped_allocator>
#include <system_error>
#include <thread>
#include <tuple>
#include <typeindex>
#include <type_traits>
#include <unordered_map>
#include <unordered_set>
#endif
```