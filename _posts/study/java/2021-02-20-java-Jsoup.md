---
layout: post
title:  "[Java] Jsoup"
subtitle:   "Jsoup 이란?"
date: 2021-02-20 01:38:46 +0900
categories: study
tags: java
comments: true
---
# Jsoup

### Jsoup 이란?

- Java로 만들어진 HTML 파서 라이브러리이다.
- URL, 파일, 문자열을 Source로 HTML을 파싱할 수 있다.
- DOM 구조 추적 및 CSS 선택자를 사용해 데이터를 추출할 수 있다.
- 문서 내 HTML 요소, 속성, 텍스트를 조작할 수 있다.
- 정돈된 형태의 html을 출력할 수 있다.

<br/>

### Jsoup 주요 요소

* Document
  * Jsoup을 통해 얻어온 최상위 노드
* Element
  * Document가 가지는 HTML 요소
* Elements
  * Element의 집합. 반복문 사용이 가능
* Connection
  * 연결을 위한 정보를 담고있는 객체
* Response
  * Jsoup으로 URL에 접속해 얻어온 결과로, Document와 다르게 status의 코드, 메시지, charset 등의 헤더 메시지와 쿠키 등도 포함한다.

<br/>

### 사용 준비

* jar 파일을 다운로드 받아서 class path에 추가하는 방법도 있다.
* Maven을 사용중이라면, 다음 의존성을 추가한다.

```xml
<dependency>
	<groupId>org.jsoup</groupId>
	<artifactId>jsoup</artifactId>
	<version>1.10.3</version>
</dependency>
```

* Gradle을 사용중이라면, build.gradle 파일에 다음 의존성을 추가한다.

```java
implementation 'org.jsoup:jsoup:1.13.1'
```

<br/>

### 수행 과정

* Jsoup을 사용해 파싱하는 과정은 이렇다.
  * Connection 객체를 통해 URL에 접속한다.
    * 또는, local file이나 문자열에 접근한다.
  * Response 객체에서 세션 ID 등의 쿠키, HTML Document를 얻어온다.
  * Document의 Element들을 파싱한다.
* 파싱의 대상은 string, local file, URL 등을 source로 할 수 있다.

<br/>

### 구문 분석(파싱)

#### 1. URL에서 문서 파싱

* 웹에서 HTML 문서를 가져와서 구문을 분석하고, 파싱하는 방법

``` java
Document doc = Jsoup.connect("http://example.com/").get();
String title = doc.title();
```

* **Jsoup.connect(String url)**
  * URL을 인자로 주어 해당 주소에 접속하여, Response를 반환한다.
  * 이어서 Response의 .get() 메소드를 통해 리턴값으로 Document를 얻어온다.
  * 이를 축약하여 바로 connect().get() 으로 사용할 수도 있다.

```java
Document doc = Jsoup.connect("http://example.com")
  .data("query", "Java")
  .userAgent("Mozilla")
  .cookie("auth", "token")
  .timeout(3000)
  .post();
```

* URL을 가져오는 동안 오류 발생을 방지하기 위한 IOException을 처리해주어야 한다.
* 또한, Builder 패턴 형식으로 여러 정보들을 직접 넣어 .post() 메소드를 이용해 Document를 얻을 수도 있다.
  * 이 방법은 웹 URL(http, https)만 지원한다.
  * 파일에서 로드하는 경우, 2번의 parse(File in, String charsetName) 을 사용한다.
* 그 후, 이 Document 객체를 이용해 원하는 Element 등을 파싱하여 사용한다.

<br/>

#### 2. 파일에서 문서 파싱

* HTML이 포함 된 파일이 디스크에 존재할 경우, 해당 경로를 이용해 데이터를 파싱하는 방법

```java
File input = new File("/tmp/input.html");
Document doc = Jsoup.parse(input, "UTF-8", "http://example.com/");
```

* **Jsoup.parse(File in, String charsetName, String baseUri)**
  * 파일 경로를 인자로 하여 파싱한다.
* 파일을 읽어오는 동안 오류 발생을 방지하기 위한 IOException을 처리해주어야 한다.

<br/>

#### 3. 문자열에서 문서 파싱

```java
String html = "<html><head><title>First parse</title></head>"
  + "<body><p>Parsed HTML into a doc.</p></body></html>";
Document doc = Jsoup.parse(html);
```

* **Jsoup.parse(String html)**
  * string 형태로 존재하는 html에서 Document 객체를 얻어오기 위한 메소드
* **Jsoup.parse(String html, String baseUri)**
  * 페이지가 웹에서 왔고, 절대 URL을 통해 얻기 위하는 경우 사용하는 메소드

<br/>

#### 4. 본문 일부를 가진 문자열에서 파싱

```java
String html = "<div><p>Lorem ipsum.</p>";
Document doc = Jsoup.parseBodyFragment(html);
Element body = doc.body();
```

* **Jsoup.parseBodyFragment(String html)**
  * 이 메소드는, 구문 분석된 HTML을 body 요소에 삽입한다.
  * 즉, **Jsoup.parse(String html)** 방법을 사용하면 일반적인 파싱과 동일한 결과를 얻지만, 이 메소드를 쓰면 사용자가 제공한 HTML이 body의 요소로 구문 분석 되어 반환한다.

<br/>

### 데이터 추출 - DOM 메소드를 이용한 문서 탐색

* 일반적으로 DOM Tree 구조를 이용한 탐색 방법을 사용한다.

```java
File input = new File("/tmp/input.html");
Document doc = Jsoup.parse(input, "UTF-8", "http://example.com/");

Element content = doc.getElementById("content");
Elements links = content.getElementsByTag("a");
for (Element link : links) {
  String linkHref = link.attr("href");
  String linkText = link.text();
}
```

<br/>

#### 요소 찾기

* getElementById(String id)
* getElementsByTag(String tag)
* getElementsByClass(String className)
* getElementsByAttribute(String key) (및 관련 방법)
* 요소 형제
  * siblingElements()
  * firstElementSibling()
  * lastElementSibling()
  * extElementSibling()
  * previousElementSibling()
* 그래프
  * parent()
  * children()
  * child(int index)

<br/>

#### 요소 데이터

* 속성 가져 오기 및 설정
  * attr(String key)
  * attr(String key, String value)
* 모든 속성 얻기
  * attributes()
* id()
* className()
* classNames()
* 텍스트 내용 가져 오기 및 설정
  * text()
  * text(String value)
* 내부 HTML 콘텐츠 가져 오기 및 설정
  * html()
  * html(String value)
* 외부 HTML 값 얻기
  * outerHtml() 
* 데이터 콘텐츠 (예 : script및 style태그) 가져 오기
  * data()
* tag()
* tagName()

<br/>

#### HTML 및 텍스트 조작

* append(String html)
* prepend(String html)
* appendText(String text)
* prependText(String text)
* appendElement(String tagName)
* prependElement(String tagName)
* html(String value)

<br/>

### 데이터 추출 - 선택자 구문을 이용한 문서 탐색

[공식 레퍼런스 참고](https://jsoup.org/cookbook/extracting-data/selector-syntax)

<br/>

<br/>

* Jsoup을 이용해 HTML 페이지를 파싱해서 데이터를 추출할 수도 있고, 데이터를 수정할 수도 있다.

<br/>

#### 그 외 레퍼런스

[참고 - tistory 블로그1](https://offbyone.tistory.com/116)

[참고 - tistory 블로그2](https://partnerjun.tistory.com/42)

[참고 - tistory 블로그3](https://hyeran-story.tistory.com/120)

[공식 레퍼런스](https://jsoup.org/)

