# Chap 07. 정규화

* Relational data model의 설계, 디자인에만 해당되는 이야기이다.
* 정규화는 이상현상을 없애며 데이터를 설계하는 방법을 말한다.

<br/>

<br/>

## 01 이상현상

* 이상현상(anomaly)

### 1. 이상현상의 개념

* **잘못 설계된 데이터베이스**가 어떤 이상현상(anomaly)을 일으키는지 알아보기
* (보통 아래 표를 정상적으로 설계하면 **학생정보 | 강좌정보** 로 나누어 설계한다)

<br/>

![image-20200615131558271](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615131558271.png)

* **삭제이상** (**deletion anomaly**) : 투플 삭제 시 같이 저장된 다른 정보까지 연쇄적으로 삭제되는 현상
  * 연쇄삭제(triggered deletion) 문제 발생
  * **마지막 한 개의 정보가 삭제되는 경우**일 때 발생
* **삽입이상** (**insertion anomaly**) : 투플 삽입 시 특정 속성에 해당하는 값이 없어 NULL 값을 입력해야 하는 현상
  * NULL 값 문제 발생
  * 위 경우 PK는 (<u>학번, 강좌이름</u>) 이기 때문에 **개체 무결성** 제약조건 (기본키 제약)에 의해 **NULL값을 허용하지 않아** 문제 발생
* **수정이상** (**update anomlay**) : **튜플 수정 시 중복**된 데이터의 일부만 수정되어 **데이터의 불일치**  문제가 일어나는 현상
  * **불일치**(inconsistency) 문제 발생
  * 학생부분과 강좌 부분의 불필요하게 많은 정보를 한 곳에 같이 묶어놓았기 때문에, 중복이 발생한다. 따라서 정보가 수정되면 해당 튜플들의 값 역시 하나 하나 찾아서 수정해줘야 하는 문제가 생긴다.
  * 해당 하나의 튜플 값만 수정했기 때문에 다른 

<br/>

![image-20200615131857562](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615131857562.png)

<br/>

## 2. 이상현상의 예

### 잘못 설계된 계절학기 수강 테이블

![image-20200615135614571](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615135614571.png)

<br/>

![image-20200615135627442](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615135627442.png)

<br/>

![image-20200615135649513](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615135649513.png)

<br/>

![image-20200615135658812](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615135658812.png)

<br/>

![image-20200615135714424](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615135714424.png)

<br/>

![image-20200615135729918](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615135729918.png)

<br/>

### 수정된 계절학기 수강 테이블

![image-20200615140216686](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615140216686.png)

<br/>

<br/>

## 02 함수 종속성

* functional dependency

<br/>

### 1. 함수 종속성의 개념

* 학생수강성적 릴레이션의 각 **속성 사이에는 의존성**이 존재함.
* 어떤 속성 **A의 값을 알면** 다른 속성 **B의 값이 유일하게 정해지는 의존 관계**를
  * ‘**속성 B는 속성 A에 종속한다 (B is functionally dependent on A)**’ 혹은
  * ‘**속성 A는 속성 B를 결정한다 (A determine B)**’라고 함.
* ‘A → B’로 표기하며, A를 B의 결정자라고 함.
* **함수 종속성의 관심사**
  * **A를 넣었는데 B와 D가 나오면 안된다**.
  * 즉, A를 넣었을 때 B만 나와야지 함수 종속성이 성립한다.
  * A를 넣었을 때와 C를 넣었을 때 B가 나오는 건 전혀 상관이 없다.

![image-20200615150939107](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615150939107.png)

<br/>

* 학생수강성적 릴레이션에서 종속관계(**FD**)에 있는 예
  * 학생번호 → 학생이름
  * 학생번호 → 주소
  * 강좌이름 → 강의실
  * 학과 → 학과사무실
* 종속하지 않는 예
  * 학생이름 → 강좌이름
  * 학과 → 학생번호

<br/>

![image-20200615151542435](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615151542435.png)

* 함수 종속성(FD, Functional Dependency)
  * 릴레이션 R과 R에 속하는 속성의 집합 X, Y가 있을 때, **X 각각의 값이 Y의 값 한 개와 대응**이 될 때 ‘**X는 Y를 함수적으로 결정한다**’라고 하고 **X→Y**로 표기함. 이때 X를 결정자(determinant)라고 하고, Y를 종속 속성(dependent attribute)이라고 함. 함수 종속성은 보통 릴레이션 설계 때 속성의 의미로부터 정해짐.

<br/>

### 2. 함수 종속성 다이어그램(functional dependency diagram)

* 함수 종속성을 나타내는 표기법
  * 릴레이션의 속성 : 직사각형
  * 속성 간의 함수 종속성 : 화살표
  * 복합 속성 : 직사각형으로 묶어서 그림

![image-20200615151930007](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615151930007.png)

<br/>

### 3. 함수 종속성 규칙

<br/>

![image-20200615152051290](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615152051290.png)

<br/>

### 4. 함수 종속성과 기본키

* 릴레이션의 함수 종속성을 파악하기 위해서는 우선 기본키를 찾아야 함.
* 기본키가 함수 종속성에서 어떤 역할을 하는지 알면 이상현상을 제거하는 정규화 과정을 쉽게 이해할 수 있음

![image-20200615152658828](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615152658828.png)

* **K 속성이 릴레이션의 모든 속성을 functionally determin 하면, K는 기본키이다.**
* **K 속성이 기본키이면, 모든 속성을 functionally determin 해야한다.**
* 풀어서 쓰면
  * K -> R
  * K -> K, A1, A2, A3, ..., An
  * K -> K, K -> A1, K -> A2, K -> A3, ..., K -> An
  * 세 경우가 같은 의미이다.

<br/>

* 예) 이름이 같은 학생이 없다고 가정하면, ‘이름 → 학과, 이름 → 주소, 이름 → 취득학점’이므로  ‘이름 → 이름, 학과, 주소, 취득학점’이 성립한다. 즉 이름 속성이 학생 릴레이션의 전체를 결정함.

![image-20200615153553864](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615153553864.png)

<br/>

### 5. 이상현상과 결정자

* 이상현상은 한 개의 릴레이션에 두 개 이상의 정보가 포함되어 있을 때 나타남.
  * **기본키가 아니면서 결정자인 속성이 있을 때** 발생한다.
  * (**기본키를 모두 결정자인 속성으로 만드는 것이 정규화의 깊은 단계**)
* 학생수강성적 릴레이션의 경우 학생 정보(학생번호, 학생이름, 주소, 학과)와 강좌 정보(강좌이름, 강의실)가 한 릴레이션에 포함되어서 이상현상이 나타남.
  * (학과, 학생번호, 강좌이름은 기본키가 아니면서 결정자인 예이다)

![image-20200615153653795](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615153653795.png)

<br/>

* 이상현상을 없애려면 릴레이션을 분해한다. 
* (학과, 학과사무실) 속성을 학생수강성적 릴레이션에서 분리하는 예 

![image-20200615153940474](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615153940474.png)

<br/>

* 릴레이션의 분해(계속….) 

![image-20200615154005112](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615154005112.png)

* 이렇게 통합된 하나의 릴레이션을 네 개의 릴레이션으로 쪼개는 분해 과정을 거치면 모두가 **행복**해진다! 

<br/>

### 따라서 Nomalization (정규화)이란 ★★★

*  이론적으로, 몽땅 **하나로 묶인 하나의 릴레이션만 존재한다고 가정**한 후, 분해하는 과정.
* **Relation**을 **Decomposition**(분해) 하는 과정이다.
* 이를 통해 **Anomaly **(이상현상)을 없애는 과정이다.
* 이때, **Functional dependency** (함수적 종속성)을 이용해 정규화 과정을 수행한다.

<br/>

* 학생수강성적 릴레이션에서 부분 릴레이션을 분해하기
  * 분해할 때 부분 릴레이션의 결정자는 원래 릴레이션에 남겨두어야 함.
  * 그래야 분해된 부분 릴레이션이 원래 릴레이션과 관계를 형성할 수 있음.

<br/>

* 1단계 : 학생수강성적 릴레이션에서 (강좌이름, 강의실)을 분리
  * 학생수강성적1(학생번호, 학생이름, 학과, 주소, 강좌이름, 성적, 학과사무실)
  * 강의실(강좌이름, 강의실)
* 2단계 : 학생수강성적1 릴레이션에서 (학생번호, 강좌이름, 성적)을 분리
  * 학생학과(학생번호, 학생이름, 학과, 주소, 학과사무실)
  * 학생성적(학생번호, 강좌이름, 성적)
  * 강의실(강좌이름, 강의실)
* 3단계 : 학생학과 릴레이션에서 (학과, 학과사무실)을 분리
  * 학생(학생번호, 학생이름, 학과, 주소)
  * 학과(학과, 학과사무실)
  * 학생성적(학생번호, 강좌이름, 성적)	
  * 강의실(강좌이름, 강의실)

<br/>

![image-20200615155946971](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615155946971.png)

<br/>

### 6. 함수 종속성 예제

* 함수 종속성은 보통 릴레이션을 설계할 때 속성의 의미로부터 정해지지만, 역으로 릴레이션에 저장된 속성 값으로부터 추정할 수 있음.

![image-20200615160053351](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615160053351.png)

<br/>

![image-20200615160620733](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615160620733.png)

### 주의할 점 ★★★

* 왼쪽 Attribute 여러개랑 오른쪽 Attribute 여러개 있는 것은 의미가 전혀 다르다!!
* 왼쪽은 여러개 합친 것이 개별적인 의미를 가지지만, 오른쪽은 여러개 합친 것이 집합처럼 여러개를 모아놓은 것과 같다.
  * [AB -> C] != [A -> C], [B -> C]
  * [AB -> CD] == [AB -> C], [AB -> D]

<br/>

<br/>

## 03 정규화 (Nomalization)

### 1. 정규화 과정

* **이상현상**이 발생하는 릴레이션을 **분해**하여 이상현상을 **없애는 과정**
* 이상현상이 있는 릴레이션은 이상현상을 일으키는 **함수 종속성의 유형에 따라** 등급을 구분 가능.
* 릴레이션은 정규형 개념으로 구분하며, 정규형이 높을수록 이상현상은 줄어듦.

![image-20200615162512640](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615162512640.png)

* 현업에서는 3정규형까지 사용한다고 한다.

<br/>

#### 제 1정규형

* 릴레이션 R의 **모든 속성 값이 원자값**을 가지면 **제 1정규형**이라고 함.
* 제 1정규형으로 변환
  * 고객취미들(이름, 취미들) 릴레이션을 고객취미(이름, 취미) 릴레이션으로 바꾸어 저장하면 제 1정규형을 만족한다.

![image-20200615162653491](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615162653491.png)

* (<u>이름, 취미</u>) 두 속성이 PK를 이룬다.

<br/>

#### 제 2정규형 (2nd NF)

* 릴레이션 R이 **제 1정규형**이고 **기본키가 아닌 속성이 기본키에 완전 함수 종속**일 때 제 **2정규형**이라고 함.
* **완전 함수 종속**(full functional dependency)
  * **A와 B가 릴레이션 R의 속성**이고
  * **A → B 종속성이 성립**할 때
  * **B가 A의 속성 전체에 함수 종속**하고
  * **부분 집합 속성에 함수 종속하지 않을 경우** 완전 함수 종속라고 함.
* 예)
  * '**성적**'은, '**학번 강좌**'에 완전함수종속이다. (Yes)
  * '**성적**'은, '**학번 강좌**'에 부분함수종속이다. (No)
  * '**강의실**'은, '**학번 강좌**'에 부분함수종속이다. (Yes)
  * '**강의실**'은, '**학번 강좌**'에 완전함수종속이다. (No)

![image-20200615162922034](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615162922034.png)

* 위의 경우, **삭제이상** (**deletion anomaly**), **삽입이상** (**insertion anomaly**), **수정이상** (**update anomaly**) 세 개의 anomaly가 발생하게 됨.
* **부분 함수 종속성**(Partial FD)이 존재하기 때문에 **2NF가 안된다**.

<br/>

* 제 2정규형으로 변환
  * 수강강좌 릴레이션에서 이상현상을 일으키는(강좌이름, 강의실)을 분해함.

![image-20200615165518197](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615165518197.png)

* **부분함수종속성이 존재하지 않기 때문에, 2NF가 된다!**
* 근데, 묶여있는게 있을 경우에만 2NF인지 아닌지 판단하는 의미가 있다.
* 왜냐면, 키가 단일값으로 이루어져 있다면 비교할 것도 없이 '부분'이라는 것이 없으니까.

<br/>

#### 제 3정규형 (3rd NF)

* 릴레이션 R이 **제 2정규형**이고 **기본키가 아닌 속성**이 **기본키에 비이행적**(non-transitive)**으로 종속할 때**(직접 종속) 제 3정규형이라고 함.
* **이행적 종속**이란 **A → B, B → C**가 성립할 때 **A → C**가 성립되는 함수 종속성

![image-20200615170320119](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615170320119.png)

* 위의 경우 기본키 학생번호에, 기본키가 아닌 수강료 속성이 결정되는 이행적 종속관계가 존재하기 때문에 3NF가 성립될 수 없다.
* 또한 위의 경우에도 마찬가지로, **삭제이상** (**deletion anomaly**), **삽입이상** (**insertion anomaly**), **수정이상** (**update anomaly**) 세 개의 anomaly가 발생하게 됨.
* 2 정규형까지 **이상 현상**이 지속적으로 발생한다.

<br/>

* 제 3정규형으로 변환
  * 계절학기 릴레이션에서 이상현상을 일으키는 (강좌이름, 수강료)를 분해함

![image-20200615171144733](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615171144733.png)

* 2개 짜리 부터는 더이상 anomaly가 존재하지 않는다.

<br/>

#### 잠시 정리

* 1NF
  * 에서 **Partial functional dependency** (**부분함수종속성**)를 제거한 것이
* 2NF
  * 에서 **Transitive dependency** (**이행적 종속**)를 제거한 것이
* 3NF

<br/>

#### BCNF

* Boyce Codd Normal Form

* 릴레이션 R에서 **함수 종속성 X → Y**가 성립할 때 **모든 결정자 X**가 **후보키**이면 **BCNF 정규형**이라고 함.

![image-20200615204135142](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615204135142.png)

* 위의 경우, 1NF 2NF 3NF 모두 성립한다.
* 학생번호, 특강이름 -> 교수 -> 특강이름 이라서 transitive 하지 않나? 할 수 있지만, **기본키가 아닌 속성이 기본키에 대해 종속**일 경우만 안되는 것이므로, 위의 경우는 괜찮다. **기본키가 기본키에 종속적**인 경우니까.
* 하지만 BCNF가 되려면 수정이 필요하다.

<br/>

* BCNF 정규형으로 변환
  * 특강수강 릴레이션에서 이상현상을 일으키는 (교수, 특강이름)을 분해함.

![image-20200615205153426](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615205153426.png)

<br/>

### 2. 무손실 분해

<br/>

### 3. 정규화 정리

* 대부분의 릴레이션은 BCNF까지 정규화하면 실제적인 이상현상이 없어지기 때문에 보통 BCNF까지 정규화를 진행함.

![image-20200615205256108](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615205256108.png)

<br/>

![image-20200615205529186](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200615205529186.png)

<br/>

<br/>

## 04 정규화 연습(부동산 데이터베이스)

<br/>

<br/>

<br/>

