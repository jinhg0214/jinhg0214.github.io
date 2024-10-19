---
title: "GitHub Actions 'Self-hosted Runner' 오류와 해결법"
date: 2024-10-19 10:00:00 +0900
categories: [IT, Web]  # 최대 2개 가능
tags: [jekyll, github, cicd]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://static1.makeuseofimages.com/wordpress/wp-content/uploads/2015/06/windows-batch-files.jpg"
    alt: 
---

포스팅글올 깃페이지에 올리는데 자꾸 글이 안뜨는 문제가 발생했다

뭐가 문제인가 싶어서 깃 CI/CD를 뒤져보니 빌드가 실패하는 상황

![image](https://github.com/user-attachments/assets/59bcc695-5200-44ae-9d3e-851dd27a2f34)

Github Actions 환경에서 CI/CD 파이프라인 관련 문제인것 같아서 찾아봤다

## 원인
---

자체 호스팅 러너인 `current runner(ubuntu-24.04-x64)`를 사용 중에

Ruby의 버전이 호환되지 않아서 발생하는 문제로 보인다

Ruby 버전을 업데이트해주면 해결 가능하다고 함

## 해결법
---

1. jekyll.yml 파일 수정해보기

`.github/workflows/jekyll.yml` 의 Ruby 버전을 '3.1'에서 '3.2'로 수정하고, 

uses도 `uses: ruby/setup-ruby@55283cc23133118229fd3f97f9336ee23a179fcf # v1.146.0`에서 

`uses: ruby/setup-ruby@086ffb1a2090c870a3f881cc91ea83aa4243d408 # v1.195.0`로 수정한다

```
jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Setup Ruby
        uses: ruby/setup-ruby@086ffb1a2090c870a3f881cc91ea83aa4243d408 # v1.195.0
        with:
          ruby-version: '3.2' # Not needed with a .ruby-version file
          bundler-cache: true # runs 'bundle install' and caches installed gems automatically
          cache-version: 0 # Increment this number if you need to re-download cached gems
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Build with Jekyll
        # Outputs to the './_site' directory by default
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
```

`.github/workflows/pages-deploy.yml.hook` 의 버전도 마찬가지로 수정해본다

```
- name: Setup Ruby
    uses: ruby/setup-ruby@v1
    with:
        ruby-version: 3.2
        bundler-cache: true
```

![image](https://github.com/user-attachments/assets/94865283-20ec-41aa-896d-3298a5d1e062)

이렇게 수정해주니 잘 빌드 된다

## 참조
---

[Build error at Setup Ruby, need help](https://talk.jekyllrb.com/t/build-error-at-setup-ruby-need-help/8791)

[Faild in ubuntu-24.04 runner #595](https://github.com/ruby/setup-ruby/issues/595) 
