---
layout: post
title:  "[Java] Design Pattern"
subtitle:   "디자인 패턴이란?"
date: 2021-03-09 23:54:46 +0900
categories: study
tags: java
comments: true
---
# Design Pattern

### 디자인 패턴이란?

* 프로그램을 개발할 때 자주 나타나는 문제를 해결하기 위한 노하우를 여러 유형으로 나누어 재사용에 용이하도록 특정 규약을 묶어 정리한 것으로, 세부적인 구현 방안을 설계할 때 참조할 수 있는 검증되고 안정된 해결 방식 또는 예제를 말한다.
* 즉, 구조적인 문제 해결을 위해 참고할 수 있는 바이블 정도로 이해하면 될 것 같다.
* '바퀴를 다시 발명하지 마라'는 말을 예시로 드는데, 문제 발생 시 새로운 해결책의 구상보다, 문제에 해당하는 디자인 패턴을 참고해서 적용하는 것이 효율적이라는 의미라고 한다.

<br/>

### 디자인 패턴의 종류

#### GoF의 디자인 패턴

* 오늘날에도 소프트웨어 공학이나 현업에서 가장 많이 사용되는 디자인 패턴이다.
* 생성(Creational) 5개, 구조(Structural) 7개, 행위(Behavioral) 11개로 23개의 패턴과 세 가지로 분류한다.

<br/>

#### 생성(Creational) 패턴

* 어떤 Concrete Class를 사용하는지에 대한 정보를 캡슐화 한다.
* 클래스의 인스턴스들이 어떻게 만들고 어떻게 결합하는지에 대한 부분을 완전히 가려준다.
  * 즉, 객체 생성과 관련된 정보들을 캡슐화 하여 특정 객체의 생성 및 변경이 있더라도 구조에 큰 영향을 받지 않도록 유연성을 제공하는 패턴이다.
* 생성 패턴의 종류
  * [Builder 패턴]()

<br/>

#### 구조(Structural) 패턴

* 

<br/>

#### 행위(Behavioral) 패턴

* 

<br/>

<br/>

### Builder 패턴

* 빌더 패턴은, 인자가 많은 생성자나 정적 팩터리가 필요한 클래스를 설계할 때, 특히 대부분의 인자가 선택적 인자인 상황에 유용하다. (Optional 한 속성을 많이 가질 때 유용하다)
  * 많은 인자를 넘겨줄 때, 타입이나 순서 등의 관리가 어려워져 에러 발생 확률이 높아진다.
  * 또한, 필요 없는 인자들에 대해서 일일이 null 값을 넘겨줘야 하는 경우가 생길 수 있다.
  * 생성해야 하는 sub class가 무거워지고 복잡해지면서, 팩토리 클래스도 복잡해진다.
* 이런 문제들의 해결을 위해, 별도의 Bulider 클래스를 만들어 필수 값은 생성자를 통해, Optional 값들은 메소드를 통해 값을 입력받은 후, build() 메소드를 통해 하나의 객체를 반환하는 방식을 말한다.
* 즉, 복잡한 객체를 생성하는 방법과 표현하는 방법을 정의하는 클래스를 별도로 분리하여 서로 다른 표현이라도 이를 생성할 수 있는 동일한 절차를 제공하는 패턴을 말한다.

<br/>

#### 예제 코드

```java
@Getter
@Builder
@ToString
public class Param {
    @Builder.Default
    private String url="URL";
    private String id;
    @Builder.Default
    private int tabPage=0;
}

...
    
private Element getElement(String storeId, int page) throws IOException {
        String url = makeUrl(Param.builder()
                .id(storeId)
                .tabPage(page)
                .build());
}
```

* PlaceInfoParam 이라는 클래스의 필드로는 url, id, tabPage 등이 있다.
* 해당 클래스를 new 생성자로 인자를 하나하나 넘겨주는 방법이 아니라 Builder 패턴을 이용한 생성 방법이다.
* 위와 같이 나열 형식으로 인자를 주거나, Default 가 존재하는 등 선택적 인자는 생략해서 생성한다.
* 마지막에는 build() 메소드를 이용해 모든 인자가 필드로 대입된 객체를 생성해 반환한다.
* **어노테이션 @Builder 를 추가함으로서, 자동으로 해당 코드를 추가해준다.**
* 구체적인 코드 설명을 복습하려면 [빌더 패턴 티스토리 블로그](https://readystory.tistory.com/121)와 맨 아래 링크들을 참조해보자.

<br/>

<br/>

[디자인 패턴](https://gmlwjd9405.github.io/2018/07/06/design-pattern.html)

[디자인 패턴 티스토리 블로그](https://readystory.tistory.com/114)

[빌더 패턴 깃 블로그](https://johngrib.github.io/wiki/builder-pattern/)

[위키백과](https://ko.wikipedia.org/wiki/%EB%94%94%EC%9E%90%EC%9D%B8_%ED%8C%A8%ED%84%B4#%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99%EC%97%90%EC%84%9C%EC%9D%98_%EB%94%94%EC%9E%90%EC%9D%B8_%ED%8C%A8%ED%84%B4)

[Effective Java](http://book.interpark.com/product/BookDisplay.do?_method=detail&sc.prdNo=294626264&gclid=Cj0KCQiA7NKBBhDBARIsAHbXCB4sGlFHLPOTS57buiXjONgFzMxgi1LjPNM3DixzLzbJ_TalI8v6ZeoaAoKiEALw_wcB)