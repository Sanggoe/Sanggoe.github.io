---
layout: post
title:  "[C] File division complie"
subtitle:   "파일 분할 컴파일이란?"
date: 2020-05-15 01:38:46 +0900
categories: study
tags: c
comments: true
---

# 파일 분할 컴파일

#### 파일 분할 컴파일이란?

코드를 작성하다보면, 실제 현업에서나 프로젝트를 할 경우 하나의 소스파일에 만단위 이상의 줄이 넘어가는 경우가 발생할 수 있다. 그런데 함께 작업을 하는 협업의 경우 처음부터 같이 시작한 사람이 아니라 중간에 합류한 사람이라면 어떨까? 하나의 소스파일에 다 담아 쓴 경우, 코드를 분석하는데만 너무 많은 시간이 소요될 것이다.

또한 C언어에서 ''함수''도 기능별로 구분하여 모듈화 하는 역할을 수행 하듯이, 코드 역시 사용자의 편의에 따라 기능별로 모듈화 하여 파일을 나누어 분할할 수 있다.

물론, 프로그램을 실행할 때는 단 하나의 main 함수만 실행이 된다. 하지만 컴퓨터가 그 main 함수를 컴파일 하여 읽으면서, 다른 곳에 쓰인 헤더 파일이라던가, 참조한 소스 파일을 전처리기 명령어(#)로 읽어 사용할 수 있다. C언어의 경우에는 이렇게 소스파일(.c), 헤더파일(.h) 등을 통해 파일을 분할하여 작성 후, 컴파일하는 방법을 선택할 수 있다. 특별한 문법이나 전문적인 기술이 아니라, 프로그램을 작성하는 하나의 방법이다.

<br/>

#### 1)  여러 파일로 분할하는 방법

##### - 소스 파일(\*.c) 여러 개로 분할하는 경우

* 특징
  * 기능별로 함수를 모듈화 하는 것처럼, 기능별 또는 임의의 기준에 따라 모듈화 하여 다른 파일로 분할하여 저장하는 방식이다.
  * 여기서는 헤더 파일에서 '선언'만 해준 부분들에 대한 구현부를 작성하기도 하고, 아예 이곳에 선언부터 구현까지 모두 해주기도 한다.
  * 소스 파일(.c) 이므로 직접 컴파일 되는 파일이다. 다만, 프로그램에는 단 하나의 main만 존재해야 하기 때문에, 주의해야 한다.
* 장점
  * 모듈화 했을 때의 장점과 같다. 이는 특히 협업을 할 때 빛을 발하는데, 분할할 때 기준을 잘 세워놓았다면 각 파일별로 기능이 잘 분리되어 독립성을 가질 수 있으므로, 나중에 유지보수를 할 때 유리하다.
  * 또한 기능별로 나누어 코드를 작성하게 되므로, 해당 기능 구현에만 집중할 수 있어 생산성 역시 높아질 수 있다.
  * 분할하여 작성한 코드가 독립적으로 만들어졌다면, 다른 프로그램에서도 재활용 할 수 있다.
  * 컴파일이 각각 이루어지므로, 에러가 발생하면 해당 파일에 대해서만 수정하면 된다.
* 단점
  * 위 특징 처럼 main이 두 번 쓰인다거나, 이미 선언된 구조체가 중복되어 선언된다거나 하는 중복 컴파일 문제가 발생할 수 있다.
  * 각 파일이 독립적으로 컴파일 된 후에 링크가 수행되기 때문에, 필요한 선언을 각 파일마다 포함해야 한다. 예를 들어, a.c 파일에서도 printf 함수를 쓰고 b.c에서도 printf 함수를 쓰기에 두 소스 코드에서 '각각' stdio.h 파일을 include 해야한다.

<br/>

##### - 헤더 파일(\*.h)로 분할하는 경우

* 특징
  * C언어를 이용해 코드를 작성할 때, 간단한 프로그램이라고 하더라도 무조건 포함하게 된다.
  * 헤더파일은 소스파일처럼 파일 자체가 '컴파일'이 되는 파일이 아니다. 
  * 하지만 #include 로 헤더파일을 읽어온 경우, 컴파일러는 불러온 모든 코드 내용을 컴파일 한다.
  * 주로는 함수의 원형이나 구조체, 공용체, 매크로 등의 선언 위주의 내용으로 작성한다.
  * 예시로 #include <stdio.h> 를 보자. standard input output 즉, 표준 입출력 함수에 대한 정의를 포함하고 있는 헤더파일이라는 의미이다.
* 장점
  * 무엇보다, 하나의 코드에 담으면 길어지게 될 코드를 분할함으로, 줄일 수 있다.
  * 마찬가지로 기능별로 나누게 된다면 생산성도 높아질 수 있고, 유지보수가 용이할 뿐 아니라  관리하기도 편하다.
  * 헤더 파일에는 선언부가 주로 정의되어 있으므로, 그 자세한 구현 내용을 알지 않더라도 '어떤 것들이 있는지' 명시적으로 한 눈에 알 수 있다는 장점이 있다.
* 단점
  * 파일 분할 시, 헤더 파일 중복오류 이슈에 대해 반드시 신경써야 한다.
  * .c 파일과는 다르게 헤더파일 자체가 컴파일 되는게 아니다보니, 각 파일들이 연결 되어있는 헤더 파일들 역시 다른 헤더파일을 포함해야 한다. 그리고 프로그램이 컴파일되고 링크되는 과정에서, 동일한 헤더 파일을 include 할 경우 컴파일 오류가 나게 된다.
  * 헤더파일 자체가 #include 하여 전처리기로 포함하는 방식인데, 컴파일러는 이 내용이 무엇이든 신경쓰지 않고 가져온 내용 모두를 컴파일한다. 즉, 헤더파일이 1000줄 짜리라고 하면 그 중 사용하는 부분이 100이라고 해도 소스파일에는 1000줄이 추가되는 것과 마찬가지라는 것이다.
  * 따라서 만약 헤더 파일을 만들어서 소스코드에 포함시키기만 해도, 프로그램 전체의 크기가 커지게 된다.
  * (찾아보니, 이에 대한 이슈는 정적 라이브러리 - Static Library, 미리 컴파일 된 헤더파일 - 라는 개념으로 해결할 수 있다고 한다.)

<br/>

##### -  파일 분할 없이 하나의 파일로 프로그램을 만들 경우

* 특징
  * 하나의 .c 코드 안에 작성한 모든 프로그램이 전부 담겨있는 상태
  * main 함수와 사용자 정의 함수 등이 전부 모여 있을 것이다.
  * 대신, 사용자가 분할한 파일이 아닌 미리 정의된 라이브러리 헤더파일 등은 포함될 것이다.
* 장점
  * 한 곳에 모여있어 '파일 관리' 가 쉽다.
  * 하나의 파일만 컴파일 하면 되므로, 컴파일 시간이 줄어든다.
  * 간단한 프로그램의 경우, 굳이 분할하여 파일의 수를 늘릴 필요 없이 하나의 파일에 모두 포함하는 것이 훨씬 간단하고 편리할 것이다.
* 단점
  * 위 두 경우의 '장점'에 해당하는 대부분의 경우들이 단일 프로그래밍 방식의 단점이 될 것이다.
  * 일단 소스코드가 너무 길어진다. 학부생 수준의 프로그램에서는 물론 감당할 수 있지만, 실제 개발 현장에 나가게 되면 천줄, 만줄은 기본으로 기나긴 코드를 하나의 프로그램에 담는다면 감당이 안될 것이다.
  * 그런 코드를 개발하는데, 중간에 팀원이 추가가 된다면? 분석만 하다가 프로젝트가 끝나지 않을까 싶다. 코드를 분석하고 파악하기가 비교적 매우 어렵다.
  * 기능별로 모여있는 것이 아니다보니, 프로그램이 매우 혼잡하고 복잡해질 가능성이 크다.

<br/>

#### 2) 분할된 소스에 정의되어 있는 함수들에서 동일한 데이터에 접근하는 방법

##### - static, extern

* 하나의 파일 단위에서 공동으로 사용하기 위한 방법이 static을 활용한 전역변수이다.
* 하지만 컴파일러는, 소스 파일 단위로 컴파일을 수행하기 때문에 다른 소스 파일에 어떤 변수가 선언되어 있는지는 알 수 없다.
* static으로 전역변수를 선언할 경우 '해당 파일' 내로 범위가 한정된다.
* 따라서 파일들이 서로 데이터를 공유하기 위해서는 extern 선언을 사용한다.
* 명시적으로 extern이라고 쓰는 것 뿐이지, 아무 키워드 없이 그냥 전역변수를 선언해 준다면, default로 모두 extern 전역변수를 뜻한다.
* 다만, 전역변수를 남용하면 파일을 분할하여 나눈 목적인 '독립성'과 외부에 내용을 일일이 보일 필요 없이 숨기는 '캡슐화'의 개념을 저버리게 되기 때문에, 해당 파일의 변수에 직접 접근하는 방법은 좋지 않은 방법이라고 한다.
* 또한 전역변수로 인해 그 용도를 명확하게 구분할 수 없어 스파게티 코드가 발생할 수 있기 때문에 남용하는 것은 자제하는게 좋다.

<br/>

##### - 다른 언어

* 객체지향 프로그래밍을 사용하는 다른 언어들의 경우, 이미 파일 분할 컴파일을 '기본 개념'으로 사용하는 경우가 많다.
* 예를 들어, Java의 경우 Class 개념으로 모듈별로 나누어 파일을 작성한다.
* 보통은 각각의 Class를 파일로 저장하고, 서로 독립적인 코드인 경우가 많다.
* 이때, 서로 참조하여 동일한 데이터에 접근하기 위해서는, 제어자인 public, private 등을 사용해 접근 권한을 제어하는 방법을 사용한다.