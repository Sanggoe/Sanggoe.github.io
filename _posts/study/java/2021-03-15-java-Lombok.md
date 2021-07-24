---
layout: post
title:  "[Java] Lombok"
subtitle:   "어노테이션 모음 라이브러리, Lombok"
date: 2021-03-15 16:31:28 +0900
categories: study
tags: java
comments: true
---
# Lombok(롬복)

### Lombok 이란?

- Java model object(DTO, VO, Domain)를 만들 때, 각 멤버 필드(property) 들에 대한 기본적인 함수들 즉, getter/setter, toString, constructor 등 불필요하게 쓰이는 반복적인 코드를 줄여주기 위해서 추가하는 어노테이션의 모음 라이브러리이다.
- 다시 말해, 어노테이션만 추가하면 각 멤버 필드에 대해 필요한 함수들을 생략할 수 있는 것이다. 특히 properties의 개수가 많을수록 정말 효율적으로 쓰일 수 있다.

<br/>

### 사용법?

* IDE의 Plug-in store 탭에서 Lombok 라이브러리를 찾아 install 하는 방법으로 간단하게 추가할 수 있다.
* Lombok.jar 파일을 직접 다운로드 받아, 해당 jar파일의 경로를 추가해 직접 라이브러리를 추가할 수도 있다.
  * [Lombok 다운로드](https://projectlombok.org/download.html)
* 사용하고자 하는 자바 파일의 상단에 @Annotation 형식으로 추가해주면 된다.
* 그러면 컴파일 시 해당 어노테이션을 읽어 직접 작성한 코드 없이도 메소드 등이 포함된 것으로 인식한다.

<br/>

### 자주 사용되는 어노테이션

#### @Data

* @Data는 다음 어노테이션들을 모두 포함한다.
  * @ToString
  * @EqualsAndHashCode
  * @Getter
  * @Setter
  * @RequiredArgsConstructor
* 주의할 점은, JPA 등의 ORM을 사용할 경우 @ToString 어노테이션으로 인해 문제가 발생한다.
* 따라서 @Data 어노테이션은 지양하는 것이 좋다.

<br/>

#### @Getter / @Setter

* 모든 필드에 대한 getter, final로 선언되지 않은 모든 필드에 대한 setter 메소드를 생성

<br/>

#### @ToString

* 모든 필드에 대해 문자열로 바꾸어 출력하는 toString() 메소드를 생성

<br/>

#### @EqualsAndHashCode

* 모든 필드에 대해 문자열로 바꾸어 출력하는 toString() 메소드를 생성

<br/>

#### @RequiredArgsConstructor

<br/>

#### @Builder

<br/>

<br/>

### Ref

[Lombok 어노테이션](https://www.daleseo.com/lombok-popular-annotations/)

[Lombok 어노테이션2](https://goddaehee.tistory.com/95)

[자주 쓰는 Lombok 어노테이션](https://velog.io/@jayjay28/%EC%9E%90%EC%A3%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-Lombok-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98)

[Lombok Reference](https://objectcomputing.com/resources/publications/sett/january-2010-reducing-boilerplate-code-with-project-lombok)