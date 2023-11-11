---
title: "Jekyll Chirpy 테마 색상 변경법"
# excerpt : 요약
date: 2023-11-01 00:10:00 +0900
# last_modified_at: 
categories: [IT, WEB] # 최대 2개 가능
tags: [gitpage, chirpy, theme, color]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
---
Gitpage를 운영하면서 Chirpy 테마를 이용중인데

색상이 너무 어두워서 색을 변경하는 방법을 알아보았다

# 1. 메인 백그라운드 색상 바꾸기
다크 테마 사용중이라면 `_sass\colors\typography-dark.scss`
밝은 테마 사용중이라면 `_sass\colors\typography-light.scss` 파일을 연다

```scss

@mixin dark-scheme {
    --main-bg: rgb(27, 27, 30);
```
부분을 원하는 색으로 변경한다

이외에도 이곳에서 테마의 원하는 부분의 색상들을 변경할 수 있다

### 사이트에 사용한 색상

#F2F3EF 흰색

#04010A 검정

#014B8D 파랑

#03588F 파랑?

#014059 파랑??

#FA7F08 주황

#F24405 빨강


# 2. 사이드바 이미지 적용

`_sass\addon\commons.scss` 파일에서 `#sidebar` 부분을 수정해준다

```scss
#sidebar {
  @include pl-pr(0);

  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  overflow-y: auto;
  width: $sidebar-width;
  z-index: 99;
  // background: var(--sidebar-bg); // 1. 이 부분을 주석처리하고 
  border-right: 1px solid var(--sidebar-border-color);

  background: url('/assets/img/sidebar2_crop.jpg'); // 2. 여기에 이미지 경로 수정 및 경로에 이미지 넣기
  background-size: 100% 100%;
  background-position: center;
    
    ...

```

# 3. 사이드바 하단 SNS 버튼들 제거하기

`_includes\sidebar.html` 파일에서 
```html
<div class="sidebar-bottom d-flex flex-wrap  align-items-center w-100">
...
</div>
```
이 블럭 주석처리해버리기 

# 4. 하단 Powered By Chirpy 지우기 

`_includes\footer.html` 파일에서

```html
  <p>
    {%- capture _platform -%}
      <a href="https://jekyllrb.com" target="_blank" rel="noopener">Jekyll</a>
    {%- endcapture -%}

    {%- capture _theme -%}
      <a href="https://github.com/cotes2020/jekyll-theme-chirpy" target="_blank" rel="noopener">Chirpy</a>
    {%- endcapture -%}

    {{ site.data.locales[include.lang].meta | replace: ':PLATFORM', _platform | replace: ':THEME', _theme }}
  </p>
```
이 블럭 주석처리해버리기