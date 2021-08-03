# Chap 06. 데이터 모델링

<br/>

<br/>

## 01 데이터 모델링의 개념

<br/>

### 1. 데이터베이스 생명주기

* life cycle : 생성되고, 소멸되기 까지의 그 과정 시간을 말한다!
* 집을 짓는다고 비유를 많이 함. 지금 우리가 배울 부분은 아래 2, 3번째라고 볼 수 있다.
  * 가능성을 파악하고
  * **분석 해보고**
  * **설계를 하고**
  * 구현을 하고
  * 유지보수를 하고

![image-20200602144725403](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602144725403.png)

<br/>

* 1단계가 개념적 모델링 (ER 다이어그램으로 바꿔 그리는 것)
  * 어떤 데이터모델을 사용하는지 여부는 관련 없다.
* 2단계가 논리적 모델링 (데이타 모델을 정해서 그 형태를 반영한 것)
* 3단계가 DB로 구현
* 현실 세계와 구현한 데이터베이스가 **일치해야 함**
  * 결국 고객의 requierment를 모두 충족 시켰느냐를 말함.

![image-20200602144945387](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602144945387.png)

<br/>

#### 데이터베이스 생명 주기(database life cycle)란?

* 데이터베이스의 생성과 운영에 관련된 특징

![image-20200602145503827](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602145503827.png)

1. 요구사항 수집 및 분석
   *  사용자들의 요구사항을 듣고 분석하여 데이터베이스 구축의 범위를 정하는 단계
2. 설계
   * 분석된 요구사항을 기초로 주요 개념과 업무 프로세스 등을 식별하고(개념적 설계), 사용하는 DBMS의 종류에 맞게 변환(논리적 설계)한 후, 데이터베이스 스키마를 도출(물리적 설계)함. (대부분 논리적 물리적 설계는 비슷. 큰 차이는 없다고 한다)
3. 구현
   * 설계 단계에서 생성한 스키마를 실제 DBMS에 적용하여 테이블 및 관련 객체(뷰, 인덱스 등)를 만듦.
4. 운영
   *  구현된 데이터베이스를 기반으로 소프트웨어를 구축하여 서비스를 제공함.
5. 감시 및 개선
   * 데이터베이스 운영에 따른 시스템의 문제를 관찰하고 데이터베이스 자체의 문제점을 파악하여 개선함.

<br/>

### 2. 데이터 모델링 과정

![image-20200602151444201](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602151444201.png)

<br/>

#### 요구사항 수집 방법

1. 실제 문서를 수집하고 분석함.
2. 담당자와의 인터뷰나 설문조사를 통해 요구사항을 직접 수렴함.
3. 비슷한 업무를 처리하는 기존의 데이터베이스를 분석함.
4. 각 업무와 연관된 모든 부분을 살펴봄.

<br/>

#### 개념적 모델링

* 요구사항을 수집하고 분석한 결과를 토대로 업무의 핵심적인 개념을 구분하고 전체적인 뼈대를 만드는 과정
* 개체(entity)를 추출하고 각 개체들 간의 관계를 정의하여 ER 다이어그램(ERD, Entity Relationship Diagram)을 만드는 과정까지를 말함.

![image-20200602152439793](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602152439793.png)

<br/>

#### 논리적 모델링

* 개념적 모델링에서 만든 ER 다이어그램을 사용하려는DBMS에 맞게 사상(매핑, mapping)하여 실제 데이터베이스로 구현하기 위한 모델을 만드는 과정

![image-20200602152743117](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602152743117.png)

<br/>

* 논리적 모델링 과정
  * 개념적 모델링에서 추출하지 않았던 상세 속성들을 모두 추출함.
  * 정규화 수행
  * 데이터 표준화 수행

<br/>

#### 물리적 모델링

* 작성된 논리적 모델을 실제 컴퓨터의 저장 장치에 저장하기 위한 물리적 구조를 정의하고 구현하는 과정
* (Create table 명령어를 직접 쓰는 과정으로, 코드를 설계하는 세부 설계라고 보면 된다)
* DBMS의 특성에 맞게 저장 구조를 정의해야 데이터베이스가 최적의 성능을 낼 수 있음.

![image-20200602152955752](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602152955752.png)

<br/>

#### 물리적 모델링 시 트랜잭션, 저장 공간 설계 측면에서 고려할 사항

* 응답시간을 최소화해야 한다.
* 얼마나 많은 트랜잭션을 동시에 발생시킬 수 있는지 검토해야 한다.
* 데이터가 저장될 공간을 효율적으로 배치해야 한다.
  * 트랜젝션 : a sequence of operation. 뭘 한다는 하나의 작업 단위.

<br/>

<br/>

## 02 ER 모델 (Entity Relationship Model)

<br/>

#### ER(Entity Relationship) 모델

* 세상의 사물을 개체(entity)와 개체 간의 관계(relationship)로 표현함.

#### 개체

* 독립적인 의미를 지니고 있는 유무형의 사물 또는 개념
* 개체의 특성을 나타내는 속성(attribute)에 의해 식별됨. 개체끼리 서로 관계를 가짐.

![image-20200602155800053](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602155800053.png)

<br/>

#### ER 다이어그램이란?

* ER 모델은 개체와 개체 간의 관계를 표준화된 그림으로 나타냄.
* ER 모델을 표현하는 도구.

![image-20200602155906022](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602155906022.png)

<br/>

### 1. 개체와 개체 타입

#### 개체(entity)란?

* 사람, 사물, 장소, 개념, 사건과 같이 유무형의 정보를 가지고 있는 독립적인 실체. (객체랑 상당히 비슷)
* 데이터베이스에서 주로 다루는 개체는 낱개로 구성된 것, 낱개가 각각 데이터 값을 가지는 것, 데이터 값이 변하는 것 등이 있음. ?
* 비슷한 속성의 **개체 타입**(entity type)을 구성하며, **개체 집합**(entity set)으로 묶임.

![image-20200602160105155](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602160105155.png)

<br/>

#### 개체 타입의 ER 다이어그램 표현

* ER 다이어그램상에서 개체 타입은 **직사각형**으로 나타냄.

![image-20200602160458529](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602160458529.png)

<br/>

#### 개체 타입의 유형

* 강한 개체(strong entity) : 다른 개체의 도움 없이 독자적으로 존재할 수 있는 개체
* 약한 개체(weak entity) : 독자적으로는 존재할 수 없고 반드시 상위 개체 타입을 가짐.

<br/>

### 2. 속성(attribute)

* 개체가 가진 성질

![image-20200602160636064](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602160636064.png)

<br/>

#### 속성의 ER 다이어그램 표현

* 속성은 기본적으로 타원으로 표현. 개체 타입을 나타내는 직사각형과 실선으로 연결됨.
* 속성의 이름은 타원의 중앙에 표기함.
* 속성이 개체를 유일하게 식별할 수 있는 키일 경우 속성 이름에 밑줄을 그음.

![image-20200602160707910](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602160707910.png)

<br/>

#### 속성의 유형

![image-20200602160830896](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602160830896.png)

* ER 모델은 개념적인 모델이기 때문에, 값을 추가하고 빼고 뭐 그러지 않는다.
* 다중값 속성은 (multi valued attribute) 여러개의 값을 갖는 속성이다. 
  * 실제 릴레이션의 구현에서는 하나 하나 따져서 분리하고 하는 과정을 거친다.
* 유도 속성은 다른 속성으로부터 값을 도출해내는 것을 말한다. (Derived attribute)
  * 필수 항목은 아니다. 따로 안만들어도 된다. 하지만 동시에 굳이 만들어도 된다.
  * SUM(price) 이라는 값을 대표적인 예로 들 수 있다.
  * SUM이라는 속성은 새로 만드는 것이 아니지만, 만약 만든다고 하면 price라는 속성에 의해 값을 도출하고 계산해 낼 수 있다는 것이다.
  * 따라서 유도속성을 해결하기 위해서는, 나이 속성을 만들지 않고 연도와 오늘날짜를 계산해서 그때그때 만 나이를 계산하든지, 아니면 속성을 만들고 1년 단위로 계산하여 갱신하든지 하면 된다.
* 복합 속성은 여러 속성으로 구성된 속성이다.
  * 만약 char string[100] 해서 값을 쭉 썼다면, 그건 복합 속성이 아니다.
  * Relational data model 에서는 허용되지 않는다.
  * 복합 속성을 해결하기 위해서는, 하위 단계를 없애든 상위 단계를 없애든 해야한다.
  * 즉, 위에서 말한 하나의 string에 값을 입력하든, 각각을 속성으로 해서 값을 가지게 하든 해야한다.

<br/>

### 3. 관계와 관계 타입

* 관계(relationship) : 개체 사이의 연관성을 나타내는 개념.
* 관계 타입(relationship type) : 개체 타입과 개체 타입 간의 연결 가능한 관계를 정의한 것이며, 관계 집합(relationship set)은 관계로 연결된 집합을 의미함.

![image-20200602162727457](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602162727457.png)

* ER 다이어그램에 그리는 것은 전부 타입간의 관계를 말한다.
* 즉, 해당 타입에 해당하는 인스턴스들이 이런 관계를 갖는다 라는 의미이다. 
* 위의 경우를 말로 정리해보자.
  * 축구아는 여자, 축구의 이해, 축구의 역사라는 세 개의 도서와, 고객 박지성 사이에는 세 개의 주문인 1번 주문, 2번 주문, 3번 주문이 존재한다.
  * 1번 주문, 2번 주문, 3번 주문 각각은 릴레이션의 인스턴스이다.
  * 즉, 위에서 말한 relationship 이라 함은, 관계 타입을 말하기도 하고 인스턴스를 말하기도 한다.
  * relationship 인스턴스들을 몇 개 모은 그룹이 relationship set 이다!
  * relationship type은 이에 대한 schema 이다!

<br/>

#### 관계 타입의 ER 다이어그램 표현

![image-20200602163832362](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602163832362.png)

<br/>

#### 관계 타입의 유형

* 차수에 따른 유형

![image-20200602180729311](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602180729311.png)

![image-20200602180746423](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602180746423.png)

* 위의 경우, 각각의 인스턴스를 말하는 것이 아니라, relationship type 이야기이다.

<br/>

★★★ 관계에서 중요한 것이 카디널리티 이다!

* 아래 경우에는 드디어 **각각의 인스턴스**들에 해당하는 이야기를 말한다.

![image-20200602181458899](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602181458899.png)

![image-20200602181756294](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602181756294.png)

![image-20200602181826352](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602181826352.png)

* 학과, 학생은 개체 타입(Entity type)을, 소속은 관계 타입(Relationship)을 의미한다.
* 학과와 학생에 속하는 각 점들은 개체 인스턴스(entity instance)를, 각 인스턴스들이 연결된 점선은 관계 인스턴스(relationship instance)를 의미한다.
* 따라서 위의 경우 위에 연결된 그림은 학과 set, relationship set, 학생 set이고 아래의 경우 type에 대한 그림이다.

<br/>

* 아래서 볼 최솟값 개념에서 접근해 보자.  **학과  -(0, *)-  소속  -(1,1)-  학생**
* 학생은 **최소한 1개 이상** 관계를 가져야 하고, **최대 1개**까지 관계를 가질 수 있다.
* 학과는 **최소한 0개**의 관계를 가져야하고, ***(또는 n)개**까지 관계를 가질 수 있다.
* **(0, 1), (0, *), (1, 1), (1, *) 네 가지 경우**가 존재한다. 완전히 다른 것을 나타낸다.

<br/>

![image-20200602181840194](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602181840194.png)

<br/>

★★★ 마찬가지로 중요한 것이 관계 대응수 이다! (참여도)

* 관계 대응수의 최솟값과 최댓값
  * 관계 대응수 1:1, 1:N, M:N에서 1, N, M은 각 개체가 관계에 참여하는 최댓값을 의미함.
  * 관계에 참여하는 개체의 **최솟값을 표시하지 않는다는 단점을 보완**하기 위해 다이어그램에서는 대응수 외에 최솟값과 최댓값을 관계실선 위에 (최솟값, 최댓값)으로 표기함.

![image-20200602182339798](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602182339798.png)

![image-20200602182350438](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602182350438.png)

* (_, _)에서 오른쪽에 있는게 카디널리티랑 관련이 있다.
* 왼쪽에 있는건 카디널리티랑 관련 없고, 참여 정도와 관련이 있다.

<br/>

####  ISA 관계

* 상위 개체 타입의 특성에 따라 하위 개체 타입이 결정되는 형태

![image-20200602201352746](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200602201352746.png)

<br/>

#### 참여 제약 조건

* 개체 집합 내 모든 개체가 관계에 참여하는지 유무에 따라 전체 참여와 부분 참여로 구분 가능.
* **전체 참여**(total participation)는 개체 집합의 모든 개체가, **부분 참여**(partical participation)는 일부만 참여함.
* 전체 참여를 (최솟값, 최댓값)으로 표현할 경우 최솟값이 1 이상으로 모두 참여한다는 뜻이고, 부분 참여는 최솟값이 0 이상이다.

![image-20200608134812948](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200608134812948.png)

* **학생**은 **수강 관계**에 [**전체참여 / 부분참여**] 한다 라고 표현
*  줄 하나의 경우 **(0, [1 / n])**, 줄 두 개의 경우 **(1, [1 / n])** 
*  즉, 괄호속의 최솟값이 바로 참여도를 이야기한다. 0이냐, 1이냐 부분 참여냐, 전체 참여냐 

<br/>

#### 역할

* 개체 타입 간의 관계를 표현할 때 각 개체들은 고유한 역할(role)을 담당함

![image-20200608135642915](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200608135642915.png)

<br/>

#### 순환적 관계

* 순환적 관계(recursive relationship) : 하나의 개체 타입이 동일한 개체 타입(자기 자신)과 순환적으로 관계를 가지는 형태.

![image-20200609101153723](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609101153723.png)

* 앞에 조인할 때 자기 자신을 조인하는 경우의 예시가 있다.

<br/>

### 4. 약한 개체 타입과 식별자

* 약한 개체(weak entity) 타입 : 상위 개체 타입이 결정되지 않으면 개별 개체를 식별할 수 없는 종속된 개체 타입 
* 자기 자신이 가진 속성만으로는 개체를 고유하게 식별할 수 없어 다른 entity type에 종속되어서 해당 개체 타입에서 빌려다가 식별하는 것이다.
* 약한 개체 타입은 독립적인 키로는 존재할 수 없지만 상위 개체 타입의 키와 결합하여 약한 개체 타입의 개별 개체를 고유하게 식별하는 속성을 **식별자(discriminator)** 혹은 부분키(partial key)라고 함. 
* (별로 많이 쓰이지 않는다고 한다. 우리가 보통 생각하는 대부분은 Strong entity type)

![image-20200609141151224](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609141151224.png)

<br/>

![image-20200609141532340](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609141532340.png)

* 직원 개체타입의 인스턴스 개수를 1000명이라고 한다면,  가족 개체타입은 직원별 가족별로 따로 있는게 아니라 2000명이라고 가정하면, 그 인원이 모두 포함된 개체집합으로 존재하는 것이다.
* 이때 동명이인이 많을 수 있으니, 가족 개체타입의 속성인 이름만으로는 기본키가 될 수 없다. 즉, 자기자신은 키를 만들 수 있는 속성을 다 갖추고 있지 않는다. 따라서 식별자로만 사용된다.
* 부양하는 관계를 맺고있는 직원 개체 타입의 직원번호 속성을 빌려 1) 직원번호와  2) 이름을 합쳐서 키로 사용을 하는 것이다.
* **ER 다이어그램에서만 표기하지 않을 뿐, 릴레이셔널 모델로 바꿀 때는 실제 식별을 위해 직원번호 속성을 추가해서 표기한다.**

<br/>

### 5. IE 표기법 (Information Engineering)

* ER 다이어그램을 더 축약하여 쉽게 표현하면 ERwin 등 소프트웨어에서 사용함.
* IE 표기법에서 개체 타입과 속성은 직사각형으로 표현함.
* Key는 상단에 표현함. 

![image-20200609101829401](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609101829401.png)

<br/>

* **IE 표기법**에서 관계는 실선 혹은 점선으로 표기함 

![image-20200609102036151](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609102036151.png)

* 필수 참여를 예시로, Chen 표기법으로 **두줄 긋기**, IE 표기법으로 **+**, 또는  **(\_ , \_)** 괄호를 이용해 표기하는 방법 등이 있다.

<br/>

* IE 표기법에서 관계(강한관계, 비식별자 관계)는 점선으로 표기함 

![image-20200609135519551](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609135519551.png)

* 상단 그림은 Chen 표기법, 아래 그림은 IE 표기법을 이용해 표현한 것이다.
* 부서는 직원이 없어도 되고(O), 직원은 부서에 반드시 소속되어야 한다(+)

<br/>

<br/>

## 03 ER 모델을 관계 데이터 모델로 사상

* 개념적 데이터 모델이 끝났으니 다음으로 논리적 모델링 단계를 거쳐야 한다~
* 완성된 ER 모델은 실제 데이터베이스로 구축하기 위해 논리적 모델링 단계를 거치는데, 이 단계에서 사상(mapping)이 이루어짐.

![image-20200609143851233](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609143851233.png)

<br/>

### 1. 개체 타입의 사상

#### [1단계] 강한(정규) 개체 타입 

* 정규 개체 타입 E의 경우 대응하는 릴레이션 R을 생성함.
* 그냥 만들면 된다.

<br/>

#### [2단계] 약한 개체 타입

* 약한 개체 타입에서 생성된 릴레이션은 자신의 키와 함께 강한 개체 타입의 키를 외래키로 사상하여 자신의 기본키를 구성함.

![image-20200609144143407](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609144143407.png)

<br/>

### 2. 관계 타입의 사상

![image-20200609144510128](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609144510128.png)

* [방법1] 오른쪽 개체 타입 E2를 기준으로 관계 R을 표현한다.
  * E1(KA1, A2)
  * E2(KA2, A4, KA1)
    * 방법 1의 경우 **1:N** 또는 **1:1** 일 경우가 가능하다.
* [방법2] 왼쪽 개체 타입 E1을 기준으로 관계 R을 표현한다.
  * E1(KA1, A2, KA2)
  * E2(KA2, A4)
    * 방법 2의 경우 **N:1** 또는 **1:1** 일 경우가 가능하다.
* [방법3] 단일 릴레이션 ER로 모두 통합하여 관계 R을 표현한다.
  * ER(KA1, A2, KA2, A4)
    * 방법 3의 경우 **1:1** 일 경우만 가능하다.
* [방법4] 개체 타입 E1, E2와 관계 타입 R을 모두 독립된 릴레이션으로 표현한다.
  * E1(KA1, A2)
  * R(KA1, KA2)
  * E2(KA2, A4)
    * 방법 4의 경우 **N:M** 일 경우만 가능하다.

<br/>

* 위와 같이, 똑같아 보이는 것에 대해서 DB를 설계할 때 각각 다른 방법으로 설계할 수 있다.
* 어떤 방법으로 설계할 것인가를 결정하는 것이 바로 DB 설계 과정이다.
* 네 가지 모두 다 의미도 방법도 다르다~!

<br/>

![image-20200609145442299](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609145442299.png)

* 위 경우처럼 1:1 관계일 경우에는, 방법 1과 2처럼 **어느 쪽의 속성을 가져다가 FK를 만들든 상관 없다.**
* 또한, 아래 방법 3처럼 사원 개체 타입과 컴퓨터 개체 타입을 합쳐서 하나로 만드는 것도 문제 없다.
* **방법 3)  사원 (번호, 이름, 컴, 사양)**  - 하지만 여기서 좋은 방법은 아니라고..

<br/>

![image-20200609145527737](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609145527737.png)

* 1:1과 다르게 **One To Many의 경우, Many 쪽에만 관계를 표시할 수 있는 속성을 추가할 수 있다.**
* 다시 말해, FK를 만드는 것이 가능하다. 학과 개체타입에 학번 속성을 가져다가 FK를 만드는 것은 불가능하다.
* **★★ 1:M에서는 Many 쪽 개체타입에다가 속성을 추가한다!!! ★★**

<br/>

![image-20200609150530288](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609150530288.png)

* ★★**Many To Many 일 경우, 관계 타입을 별도의 릴레이션으로 살린다.** ★★
* 즉, Relationship을 별도의 Relation으로 만들어준다.
* 따라서 교수에서 사번을 따오고, 과목에서 과목코드를 따와 수업이라는 릴레이션을 만든다.

<br/>

![image-20200609150619682](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609150619682.png)

* 많이 쓰이진 않는다고 한다.
* 2진 관계 세 개로 나누어서 보기도 한다.

<br/>

### 3. 다중값 속성의 사상

![image-20200609151912297](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609151912297.png)

![image-20200609152324090](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609152324090.png)

* 릴레이션의 속성은 원자값을 가져야 한다. 따라서 취미처럼 다중 값은 그대로 표현할 수 없다.
* 그러므로 먼저, 관계 데이터 모델로 만들 경우 항목의 개수를 제한하여 취미 1, 2, 3으로 만드는 방법이 있다.
* 만약 항목의 개수를 알 수 없는 경우 별도의 릴레이션을 만들어야 한다.

<br/>

<br/>

## 04. ERwin 실습

* ERwin : 데이터 모델링을 하기 위한 프로그램. IE 표기법을 지원함.

![image-20200609164226294](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200609164226294.png)

<br/>

* 뒤에 부분은 ppt로 보자

<br/>

```

```

