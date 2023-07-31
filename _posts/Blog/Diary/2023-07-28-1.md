---
layout: post
title: Github 블로그 Jekyll로 구축하기
comments: true
tags: [jekyll, github, markdown]
categories: Blog
---
Jekyll을 설치하기 위해 기본 플러그인으로 Ruby를 설치해야 한다.   
우분투에서 기본 루비를 설치해도 되지만, 버전 오류가 끝없이 나와서 rvm을 사용해 설치하는 것을 권장. 

```bash
\\curl -sSL <https://get.rvm.io> | bash -s stable
rvm install ruby-3.0.0
rvm use ruby-3.0.0
rvm --default use 3.0.0
ruby -v

gem install jekyll bundler
```

## Bundle dependency 오류 발생시 
```bash
bundle install
bundle update
```

## 로컬에서 서버 접속
```bash
bundle exec jekyll serve 
> <http://localhost:4000> 접속해서 확인

다시 로깅하는 경우에는 
> jekyll serve --skip-initial-build
```

그리고 블로그 포스팅 작성 시 제목 바로 다음줄에 마크다운 제목이 들어가면 컨티뉴 리딩이 안 뜬다. 이건 무슨 오류인지 모르겠지만... 일단 그렇다... 