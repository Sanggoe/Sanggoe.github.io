---
layout: post
title:  "[Git] Git ignore"
subtitle:   "git ignore file이란?"
date: 2021-02-25 23:16:45 +0900
categories: study
tags: git
comments: true
---
# Git ignore
### .gitignore은 무엇인가?

* git으로 버전을 관리하는 프로젝트에서, 원하지 않는 파일들을 의도적으로 무시하도록 지정하는 파일을 말한다.

<br/>

### 파일 만들기

* .gitignore를 확장자로 사용한다. 

* 항상 최상위 디렉토리에 존재해야만 한다.

<br/>

### 예시 파일

```ini
# java class files
*.class

# Ignore gradle files
.gradle/
build/

# Intellij IDEA
.idea/workspace.xml
.idea/datasources.xml
```

* .class 확장자로 끝나는 파일은 추적하지 않는다.
* .gradle/, build/ 경로에 해당하는 폴더는 추적하지 않는다.
* .idea/workspace.xml, .idea/datasources.xml 파일은 추적하지 않는다. 

<br/>

### 적용되지 않을 때, 적용하기

* 기존 프로젝트의 gitignore 파일이 적용 안될 때는, 아래 명령어를 적용시켜본 뒤, 다시 push 해본다.

```bash
git rm -r --cached .
git add .
git commit -m "reapply git ignore file"
```

* 로컬 리포지토리의 캐시를 삭제한 뒤 다시 추가하여 적용하는 방법이다.

<br/>

### 자동 생성 사이트

사용하는 OS, 개발 환경, 언어 등을 입력하면 자동으로 gitignore 파일을 생성해주는 사이트이다. 

[gitignore.io](https://www.toptal.com/developers/gitignore)

<br/>