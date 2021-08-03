

# Chap 03. SQL 기초

<br/>

<br/>

## 01 SQL 학습을 위한 준비

<br/>

#### 오라클을 설치한다.

* [설치 참고 블로그 1](https://goddaehee.tistory.com/191)
* [설치 참고 블로그 2](https://corock.tistory.com/127)

#### 설치 중 오류가 나면 골치아프다.

* [삭제 참고 블로그 1](https://wookoa.tistory.com/304)

<br/>

#### sql plus에서 자주 쓰이는 명령어

* **conn** : DB 접속
  * ex) conn scott/tiger : scott 계정에 비밀번호 tiger로 접속
* **run**, **/** : 명령어 실행
  * ex) run : 바로 전 실행했던 명령어 다시 실행
  * ex) / : run과 같은 의미
* **list** : 명령어 찾기
  * ex) list : 마지막 수행 명령어 출력
* **ed <file_name>, run <file_name>** : 메모장 이용 명령어 작성 및 실행
  * ex) ed test : test.sql 이름의 파일이 메모장을 이용해 작성할 수 있도록 열린다.
  * ex) start test : test.sql 이름에 저장된 명령어 스크립트가 실행
  * ex) @ test : start test와 같은 의미 
* **column **: 출력 모양을 조절하는 명령
  * ex) column bookname format a20 : bookname을 길이 20의 문자 포맷으로 출력
  * ex) column price format 999999 : price를 길이 6개의 숫자 포맷으로 출력

<br/><br/>

## 02 SQL 개요

<br/>

#### SQL

* Structured Query Language의 약자
* C나 Java 처럼 완전한 프로그래밍 언어는 아니다.
* Data나 meta data에 대한 생성 및 처리 문법만 가지고 있기 때문이다.

* 다른 프로그래밍 언어에 삽입하여 사용할 수 있는 데이터 sub language 형태이다.

<br/>

#### SQL과 일반 프로그래밍 언어의 차이점

| 구분    | SQL                              | 일반 프로그래밍 언어   |
| ------- | -------------------------------- | ---------------------- |
| 용도    | DB에서 Data를 추출하여 문제 해결 | 모든 문제 해결         |
| 입출력  | Input은 Table, Output도 Table    | 모든 형태의 I/O 가능   |
| 번역    | DBMS                             | Compiler               |
| 사용 예 | SELECT * <br />FROM Book         | int main() <br />{...} |

<br/>

#### SQL 필수 기능에 따른 분류

* 데이터 정의어 (**DDL**) :  테이블이나 관계의 **구조를** 생성, 수정 및 삭제하는데 사용
  * CREATE, ALTER, DROP문 등이 있음
* 데이터 조작어 (**DML**) : 테이블에 **데이터를** 검색, 삽입, 수정, 삭제하는데 사용
  * SELECT, INSERT, DELETE, UPDATE문 등이 있음
  * **SELECT문**은 특별히 **질의어**(**query**)라고 한다.
* 데이터 제어어 (**DCL**) : 데이터의 사용 권한을 관리하는데 사용
  * GRANT, REVOKE문 등이 있음

<br/>

![image-20200429141817612](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200429141817612.png)

<br/>

#### 예) query문과 relational algebra의 비교

```sql
SELECT	phone
FROM	Customer
WHERE	name='김연아'
```

##### 위 쿼리문과 관계 대수를 비교하자면,

* SELECT = 프로젝션 (projection)
* FROM = 테이블 이름 (R), 두 개 이상이면 곱하라는 의미
* WHERE = 셀렉션 (selection)

로 볼 수 있다.

<br/><br/>

## 03 데이터 조작어 DML - 검색

<br/>

#### SELECT 문의 구성 요소

![image-20200429143236222](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200429143236222.png)

* 명령어를 포함한 각 줄을 **절(clause)** 이라 표현한다.

<br/>

### 1. SELCT 문

<br/>

#### SELECT / FROM 의 기본 문법

```sql
SELECT [ALL┃DISTINCT] 속성이름(들) // default는 ALL(*) 이다.
FROM		테이블이름(들)
[WHERE		검색조건(들)]
[GROUP BY	속성이름]
[HAVING		검색조건(들)]
[ORDER BY	속성이름 [ASC┃DESC]] // default는 ASC(오름차순) 이다.
--------------------------------------------------------------------------------
[ ] : 대괄호 안의 SQL 예약어들은 선택적으로 사용한다.
| : 선택 가능한 문법들 중 한 개를 사용할 수 있다.

* 위 기호처럼 문자 자체로서가 아니라, 특정한 의미를 담는 기호들을 meta simbol 이라고 한다!
```

<br/>

#### 질의 3-1) 모든 도서의 이름과 가격을 검색하시오.

```sql
SELECT bookname, price
FROM  Book;
```

* 모든 이므로, **σ**가 필요 없다.
* **π**로 name, price를 Book에서 검색하면 된다.
* bookname과 price의 순서를 바꿔도 상관 없다.

<br/>

#### 질의 3-2) 모든 도서의 도서번호, 도서이름, 출판사, 가격을 검색하시오.

```sql
SELECT	bookid, bookname, publisher, price
FROM	Book;
--------------------------------------------------------------------------------
SELECT	*
FROM	Book;
```

* '*' 는 모든 column이라는 의미로, 같은 의미이다.

<br/>

#### 질의 3-3) 도서 테이블에 있는 모든 출판사를 검색하시오.

```sql
SELECT	publisher
FROM	Book;
--------------------------------------------------------------------------------
SELECT	DISTINCT publisher
FROM	Book;
```

* SQL문은 관계 대수와 달리, 기본적으로 중복을 제거하지 않는다.
* 즉, publisher만 출력하면 해당 column값에는 중복되어 출력되는 값들이 존재한다.
* 중복을 제거하고 싶으면 DISTINCT라는 키워드를 사용하면 된다.

<br/>

#### WHERE 조건의 기본 문법

| 술어     | 연산자                | 예                                                |
| -------- | --------------------- | ------------------------------------------------- |
| 비교     | =, <>, <, <=, >, >=   | price < 20000                                     |
| 범위     | BETWEEN               | price BETWEEN 10000 AND 20000                     |
| 집합     | IN, NOT IN            | price IN (10000, 20000, 30000)                    |
| 패턴     | LIKE (문자열에서 '=') | bookname LIKE '축구의 역사'                       |
| NULL     | IS NULL, IS NOT NULL  | price IS NULL                                     |
| 복합조건 | AND, OR, NOT          | (price < 20000) AND (bookname LIKE '축구의 역사') |

<br/>

#### 질의 3-4) 가격이 20,000원 미만인 도서를 검색하시오.

```sql
SELECT	*
FROM	Book
WHERE	price < 20000;
```

<br/>

#### 질의 3-5) 가격이 10,000원 이상 20,000 이하인 도서를 검색하시오.

```sql
SELECT	*
FROM	Book
WHERE	price BETWEEN 10000 AND 20000;
--------------------------------------------------------------------------------
SELECT	*
FROM	Book
WHERE	price >= 10000 AND price <= 20000;
```

* BETWEEN은 논리 연산자인 AND를 사용할 수 있다.
* 위와 아래의 문장은 같은 의미이다.

<br/>

#### 질의 3-6) 출판사가 ‘굿스포츠’ 혹은 ‘대한미디어’인 도서를 검색하시오.

```sql
SELECT	*
FROM	Book
WHERE	publisher IN ('굿스포츠', '대한미디어');
```

* 위 질의와 반대로, '굿스포츠' 혹은 ''대한미디어' 가 아닌 도서를 겸색할 경우
* NOT IN 으로 바꿔서 쓰면 된다.

<br>

#### 질의 3-7) ‘축구의 역사’를 출간한 출판사를 검색하시오.

```sql
SELECT	bookname, publisher
FROM	Book
WHERE	bookname LIKE '축구의 역사';
```

* LIKE는 문자열에 대한 '='와 같다.
* Java에서 문자열을 == 하는 대신 string.equal("__") 하는 것과 비슷한 뉘앙스로 보면 될 듯.

<br/>

#### 질의 3-8) 도서이름에 ‘축구’가 포함된 출판사를 검색하시오.

```sql
SELECT	bookname, publisher
FROM	Book
WHERE	bookname LIKE '%축구%';
```

* 위에서 %는, 운영체제에서 검색할 때 * 의 의미와 같다.

<br/>

#### 질의 3-9) 도서이름의 왼쪽 두 번째 위치에 ‘구’라는 문자열을 갖는 도서를 검색하시오.

```sql
SELECT	*
FROM	Book
WHERE	bookname LIKE '_구%';
```

* _ 는 1개의 문자가 있는 경우를 말한다.

<br/>

#### 와일드 문자의 종류

| 와일드 문자 | 의미                        | 사용 예                                           |
| ----------- | --------------------------- | ------------------------------------------------- |
| +           | 문자열을 연결               | '골프' + '바이블' : '골프 바이블'                 |
| %           | 0개 이상의 문자열과 일치    | '%축구%' : 축구를 포함하는 문자열                 |
| [ ]         | 1개의 문자와 일치           | '[0-5]%' : 0-5 사이 숫자로 시작하는 문자열        |
| [^]         | 1개의 문자와 불일치         | '[ ^0-5]%' : 0-5 사이 숫자로 시작하지 않는 문자열 |
| _           | 특정 위치의 1개 문자와 일치 | '_구'%' : 두 번째 위치에 '구' 가 들어가는 문자열  |

<br/>

#### 질의 3-10) 축구에 관한 도서 중 가격이 20,000원 이상인 도서를 검색하시오.

```sql
SELECT	*
FROM	Book
WHERE	bookname LIKE '%축구%' AND price >= 20000;
```

<br/>

#### 질의 3-11) 출판사가 ‘굿스포츠’ 혹은 ‘대한미디어’인 도서를 검색하시오.

```sql
SELECT	*
FROM	Book
WHERE	publisher='굿스포츠'  OR  publisher='대한미디어';
```

<br/>

#### ORDER BY절

* 정렬의 기본은 오름차순이다.
* 내림차순 정렬 시, 열 이름 다음에 **DESC** 키워드를 사용한다.

<br/>

#### 질의 3-12) 도서를 이름순으로 검색하시오.

```sql
SELECT		*
FROM		Book
ORDER BY	bookname;
```

<br/>

#### 질의 3-13) 도서를 가격순으로 검색하고, 가격이 같으면 이름순으로 검색하시오.

```sql
SELECT		*
FROM		Book
ORDER BY	price, bookname;
```

<br/>

#### 질의 3-14) 도서를 가격의 내림차순으로 검색하시오. 만약 가격이 같다면 출판사의 오름차순으로 검색한다.

```sql
SELECT		*
FROM		Book
ORDER BY	price DESC, publisher [ASC];
```

* 정렬의 Default는 오름차순(ASC)이다.
* DESC 입력 시 내림차순으로 정렬한다.

<br/>

#### 집계 함수

* 통계를 낼 때 사용한다.
* 합계를 알고싶을 때도 사용한다.

<br/>

#### 질의 3-15) 고객이 주문한 도서의 총 판매액을 구하시오.

```sql
SELECT	SUM(saleprice) 
FROM	Orders;
--------------------------------------------------------------------------------
SELECT	SUM(saleprice) AS 총매출
FROM	Orders;
```

* 의미있는 열 이름을 출력하고 싶으면, 속성 이름의 별칭을 지칭하는 **AS** 키워드를 사용할 수 있다.
* 결과 테이블 **출력 시**, 위의 경우 SUM(saleprice), 아래의 경우 총매출 이라고 출력된다.

<br/>

#### 질의 3-16) 2번 김연아 고객이 주문한 도서의 총 판매액을 구하시오.

```sql
SELECT	SUM(saleprice) AS 총매출
FROM	Orders
WHERE	custid=2;
```

<br/>

#### 질의 3-17) 고객이 주문한 도서의 총 판매액, 평균값, 최저가, 최고가를 구하시오.

```sql
SELECT	SUM(saleprice) AS Total,
        AVG(saleprice) AS Average,
        MIN(saleprice) AS Minimum,
        MAX(saleprice) AS Maximum
FROM    Orders;
```

<br/>

#### 질의 3-18) 마당서점의 도서 판매 건수를 구하시오.

```sql
SELECT	COUNT(*)
FROM    Orders;
```

<br/>

#### 집계 함수의 종류

| 집계 함수 | 문법                                       | 사용 예    |
| --------- | ------------------------------------------ | ---------- |
| SUM       | SUM([ALL \| DISTINCT] 속성이름)            | SUM(price) |
| AVG       | AVG([ALL \| DISTINCT] 속성이름)            | AVG(price) |
| COUNT     | COUNT({[[ALL \| DISTINCT] 속성이름] \| *}) | COUNT(*)   |
| MAX       | MAX([ALL \| DISTINCT] 속성이름)            | MAX(price) |
| MIN       | MIN([ALL \| DISTINCT] 속성이름)            | MIN(price) |

<br/>

### GROUP BY

* 그룹별로 알고 싶을 때 사용하는 절

<br/>

#### 질의 3-19) 고객별로 주문한 도서의 총 수량과 총 판매액을 구하시오.

```sql
SELECT		custid, COUNT(*) AS 도서수량, SUM(saleprice) AS 총액
FROM		Orders
GROUP BY	custid;
```

<br/>

#### 질의 3-20) 가격이 8,000원 이상인 도서를 구매한 고객에 대하여 고객별 주문 도서의 총 수량을 구하시오. 단, 두 권 이상 구매한 고객만 구한다.

```sql
SELECT		custid, COUNT(*) AS 도서수량
FROM		Orders
WHERE		saleprice >= 8000
GROUP BY	custid
HAVING		count(*) >= 2; // ★
```

* 일단 쿼리를 만나면, 어떤 테이블(Orders)이 필요한가? 를 알아내야 한다.

* 최종적으로 원하는 Attribute가 무엇인가? 즉, 프로젝션 할 것이 뭔가를 먼저 잘 파악한다.

  

* 조건을 반드시 잘 확인한다. 위의 경우, 각 튜플에 대한 조건이 아니라 그룹에 대한 조건이다.

* WHERE의 경우 튜플 각각에 대한 조건을 명시한다.

* HAVING의 경우 그룹 전체에 대한 조건을 명시한다.

<br/>

| **문법**          | **주의사항**                                                 |
| ----------------- | ------------------------------------------------------------ |
| GROUP BY <속성>   | GROUP BY로 투플을 그룹으로 묶은 후 SELECT 절에는<br />GROUP BY에서 사용한 <속성>과  집계함수만 나올 수 있음<br /><br />• 맞는  예<br /> SELECT     custid, SUM(saleprice)<br /> FROM       Orders  GROUP BY  custid;<br /><br />• 틀린  예 <br /> SELECT     bookid, SUM(saleprice) /* SELECT 절에 bookid 속성이  올 수 없다 */ <br /> FROM       Orders  GROUP BY  custid; |
| HAVING <검색조건> | WHERE 절과 HAVING 절이  같이 포함된 SQL 문은 검색조건이 모호해질 수 있음.  <br />HAVING 절은<br />① 반드시 GROUP BY 절과 같이 작성해야 하고<br />② WHERE 절보다 뒤에 나와야 함. <br />③ <검색조건>에는 SUM, AVG, MAX, MIN, COUNT와 같은 집계함수가 와야 함. <br />• 맞는  예 <br /> SELECT        custid, COUNT(\*) AS 도서수량 <br /> FROM           Orders<br /> WHERE         saleprice ＞= 8000<br /> GROUP BY   custid <br /> HAVING        COUNT(\*) ＞= 2;<br /><br />• 틀린  예<br />  SELECT         custid, COUNT(\*) AS 도서수량 <br />  FROM            Orders <br />  HAVING         COUNT(\*) ＞= 2  /* 순서가  틀렸다 */ <br />  WHERE         saleprice ＞= 8000 <br />  GROUP BY   custid; |

<br/>

### 조인

* 여러 개의 테이블을 합치는 과정에 쓰이는 연산
* FROM 절에서 테이블 두 개 이상 쓰면, 카티션 프로덕트를 수행하라는 의미이다.
* 아무튼 조인은 카티션 프로덕트와 셀렉션의 콤비다.

<br/>

#### 질의 3-21) 고객과 고객의 주문에 관한 데이터를 모두 보이시오.

```sql
SELECT	*
FROM    Customer, Orders
WHERE   Customer.custid = Orders.custid;
```

<br/>

#### 질의 3-22) 고객과 고객의 주문에 관한 데이터를 고객번호 순으로 정렬하여 보이시오.

```sql
SELECT	*
FROM     	Customer, Orders
WHERE    	Customer.custid = Orders.custid
ORDER BY	Customer.custid;
```

<br/>

#### 질의 3-23) 고객의 이름과 고객이 주문한 도서의 판매가격을 검색하시오.

```sql
SELECT	name, saleprice
FROM    Customer, Orders
WHERE   Customer.custid = Orders.custid;
```

<br/>

#### 질의 3-24) 고객별로 주문한 모든 도서의 총 판매액을 구하고, 고객별로 정렬하시오.

```sql
SELECT		name, SUM(saleprice)
FROM      	Customer, Orders
WHERE     	Customer.custid = Orders.custid
GROUP BY	Customer.name
ORDER BY	Customer.name;
```

<br/>

#### 질의 3-25) 고객의 이름과 고객이 주문한 도서의 이름을 구하시오.

```sql
SELECT	Customer.name, Book.bookname
FROM    Customer, Orders, Book
WHERE   Customer.custid = Orders.custid 
        AND Orders.bookid = Book.bookid;
```

<br/>

#### 질의 3-26) 가격이 20,000원인 도서를 주문한 고객의 이름과 도서의 이름을 구하시오.

```sql
SELECT	Customer.name, Book.bookname
FROM    Customer, Orders, Book
WHERE   Customer.custid = Orders.custid AND Orders.bookid = Book.bookid
        AND Book.price = 20000;
```

<br/>

#### 외부 조인

<br/>

#### 질의 3-27) 도서를 구매하지 않은 고객을 포함하여 고객의 이름과 고객이 주문한 도서의 판매가격을 구하시오.

```sql
SELECT	Customer.name, saleprice
FROM	Customer LEFT OUTER JOIN 
        Orders ON Customer.custid = Orders.custid;
```

<br/>

#### 조인 문법

![image-20200519161059923](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200519161059923.png)

<br/>

### 부속질의

* SQL문 내에 또 다른 SQL문을 작성하는 방법
* 괄호 안에 부속 질의 먼저 수행된 후 바깥의 질의가 수행되는 순서로 진행된다.

<br/>

#### 질의 3-28) 가장 비싼 도서의 이름을 보이시오.

```sql
SELECT	bookname
FROM    Book
WHERE   price = ( SELECT MAX(price)
            	  FROM Book);
```

<br/>

#### 질의 3-29) 도서를 구매한 적이 있는 고객의 이름을 검색하시오.

```sql
SELECT	name
FROM    Customer
WHERE   custid IN ( SELECT	custid
                    FROM	Orders);
```

<br/>

#### 질의 3-30) 대한미디어에서 출판한 도서를 구매한 고객의 이름을 보이시오.

```sql
SELECT	name
FROM    Customer
WHERE	custid IN ( SELECT	custid
					FROM	Orders
					WHERE	bookid IN ( SELECT	bookid
										FROM	Book
										WHERE	publisher='대한미디어'));
```

<br/>

#### 상관 부속질의

* 상관 부속질의(correlated subquery)는 상위 부속질의의 투플을 이용하여 하위 부속질의를 계산함. 즉 상위 부속질의와 하위 부속질의가 독립적이지 않고 서로 관련을 맺고 있음.

<br/>

#### 질의 3-31) 출판사별로 출판사의 평균 도서 가격보다 비싼 도서를 구하시오.

```sql
SELECT 	b1.bookname
FROM 	Book b1
WHERE 	b1.price > (SELECT 	avg(b2.price)
					FROM 	Book b2
					WHERE 	b2.publisher = b1.publisher);
```

<br/>

### 집합 연산

#### 합집합 UNION, 차집합 MINUS, 교집합 INTERSECT 

* {도서를 주문하지 않은 고객} = {모든 고객} - {도서를 주문한 고객}
* (주) **Oracle**은 차집합을 **MINUS**로 하지만 **SQL 표준**에서는 **EXCEPT** 를 사용한다.

<br/>

#### 질의 3-32) 도서를 주문하지 않은 고객의 이름을 보이시오.

```sql
SELECT 	name
FROM 	Customer
MINUS
SELECT 	name
FROM 	Customer
WHERE 	custid IN (	SELECT custid
					FROM  Orders);
---------------------------------------------------------------
SELECT 	name
FROM 	Customer
WHERE 	custid NOT IN (	SELECT custid
						FROM  Orders);
```

<br/>

#### EXISTS

* 원래 단어에서 의미하는 것과 같이 조건에 맞는 튜플이 존재하면 결과에 포함시킴. 즉 부속질의문의 어떤 행이 조건에 만족하면 참임. 반면 NOT EXISTS는 부속질의문의 모든 행이 조건에 만족하지 않을 때만 참임.

#### 질의 3-33) 주문이 있는 고객의 이름과 주소를 보이시오.

```sql
SELECT 	name, address
FROM 	Customer cs
WHERE 	EXISTS (SELECT *
				FROM  Orders od
				WHERE cs.custid = od.custid);
--------------------------------------------------------------				
SELECT 	name, address
FROM	customer 
WHERE   custid IN ( SELECT custid
                    FROM  orders
                    WHERE orders.custid = customer.custid);
```

<br/>

<br/>

## 04 데이터 정의어 DDL

<br/>

### CREATE 문

* 테이블을 구성하고, 속성과 속성에 관한 제약을 정의하며, 기본키 및 외래키를 정의하는 명령
* PRIMARY KEY는 기본키를 정할 때 사용
* FOREIGN KEY는 외래키를 지정할 때 사용
* ON UPDATE와 ON DELETE는 외래키 속성의 수정과 투플 삭제 시 동작을 나타냄.

<br/>

#### CREATE 문의 기본 문법

```sql
CREATE TABLE 테이블이름
 ( { 속성이름 데이터타입
     [NOT NULL | UNIQUE | DEFAULT 기본값 | CHECK 체크조건]
    }
     [PRIMARY KEY 속성이름(들)]
     {[FOREIGN KEY 속성이름 REFERENCES 테이블이름(속성이름)]
 	 [ON DELETE [CASCADE | SET NULL]
     }
 )
```

<br/>

#### 질의 3-34) 다음과 같은 속성을 가진 NewBook 테이블을 생성하시오, 정수형은 NUMBER를, 문자형은 가변형 문자타입인 VARCHAR2를 사용한다.

* bookid(도서번호) - NUMBER
* bookname(도서이름) – VARCHAR2(20)
* publisher(출판사) – VARCHAR2(20)
* price(가격) – NUMBER

```sql
CREATE TABLE NewBook (
  bookid		NUMBER,
  bookname		VARCHAR2(20),
  publisher		VARCHAR2(20),
  price			NUMBER);
  
--------------------------------------------------------------------------------
※ 기본키를 지정하고 싶다면 다음과 같이 생성한다.

CREATE TABLE NewBook (
 bookid		NUMBER, 		// NUMBER PRIMARY KEY, 여기에 표시해도 된다. ★
 bookname	VARCHAR2(20),
 publisher	VARCHAR2(20),
 price		NUMBER,   
 PRIMARY KEY (bookid));		// ★ 둘 중 하나 선택!
 
 -------------------------------------------------------------------------------
 ※ bookid 속성이 없어서 두 개의 속성 bookname, publisher가 기본키가 된다면
   괄호를 사용하여 복합키를 지정한다.

CREATE TABLE NewBook (
 bookname	VARCHAR2(20),
 publisher	VARCHAR2(20),
 price		NUMBER,
 PRIMARY KEY (bookname, publisher));	// ★

 -------------------------------------------------------------------------------
 * bookname은 NULL 값을 가질 수 없고, publisher는 같은 값이 있으면 안 된다.
   price에 값이 입력되지 않을 경우 기본 값 10000을 저장한다.
   또 가격은 최소 1,000원 이상으로 한다.
   
 ※ NewBook 테이블의 CREATE 문에 좀 더 복잡한 제약사항을 추가한다.
 
CREATE TABLE NewBook (
  bookname	VARCHAR(20)	NOT NULL,
  publisher	VARCHAR(20)	UNIQUE,
  price		NUMBER		DEFAULT 10000 CHECK (price > 1000),
  PRIMARY KEY (bookname, publisher));
```

<br/>

#### 질의 3-35) 다음과 같은 속성을 가진 NewCustomer 테이블을 생성하시오.

* custid(고객번호) - NUMBER, 기본키
* name(이름) – VARCHAR2(40)
* address(주소) – VARCHAR2(40)
* phone(전화번호) – VARCHAR2(30)

```sql
CREATE TABLE NewCustomer (
 custid		NUMBER  PRIMARY KEY,
 name		VARCHAR2(40),
 address	VARCHAR2(40),
 phone		VARCHAR2(30) );
```

<br/>

#### 질의 3-36) 다음과 같은 속성을 가진 NewOrders 테이블을 생성하시오.

* orderid(주문번호) - NUMBER, 기본키
* custid(고객번호) - NUMBER, NOT NULL 제약조건, 외래키(NewCustomer.custid, 연쇄삭제)
* bookid(도서번호) - NUMBER, NOT NULL 제약조건
* saleprice(판매가격) - NUMBER 
* orderdate(판매일자) - DATE

```sql
CREATE TABLE NewOrders (
 orderid 	NUMBER,
 custid 	NUMBER 	NOT NULL,
 bookid 	NUMBER 	NOT NULL,
 saleprice 	NUMBER,
 orderdate 	DATE,
 PRIMARY KEY (orderid),
 FOREIGN KEY (custid) REFERENCES NewCustomer(custid) ON DELETE CASCADE ); // ★
```

<br/>

#### 외래키 제약조건을 명시할 때

* 반드시 참조되는 테이블(부모 릴레이션)이 존재해야 하며 참조되는 테이블의 기본키여야 함.
* 외래키 지정 시 ON DELETE 또는 ON UPDATE 옵션은 참조되는 테이블의 튜플이 삭제되거나 수정될 때 취할 수 있는 동작을 지정함.
* NO ACTION은 어떠한 동작도 취하지 않음.

![image-20200519163803190](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200519163803190.png)

<br/>

### ALTER 문

* 생성된 테이블의 속성과 속성에 관한 제약을 변경하며, 기본키 및 외래키를 변경함.
* 즉, **스키마를 변경할 때 사용한다.**
* ADD, DROP은 속성을 추가하거나 제거할 때 사용함.
* MODIFY는 속성의 기본값을 설정하거나 삭제할 때 사용함.
* 그리고 ADD <제약이름>, DROP <제약이름>은 제약사항을 추가하거나 삭제할 때 사용함.

<br/>

#### ALTER 문의 기본 문법

```sql
ALTER TABLE 테이블이름
[ADD 속성이름 데이터타입]
	[DROP COLUMN 속성이름]
	[MODIFY 속성이름 데이터타입]
	[MODIFY 속성이름 [NULL┃NOT NULL]]
	[ADD PRIMARY KEY(속성이름)]
	[[ADD┃DROP] 제약이름]
```

<br/>

#### 질의 3-37) NewBook 테이블에 VARCHAR2(13)의 자료형을 가진 isbn 속성을 추가하시오.

```sql
ALTER TABLE NewBook ADD isbn VARCHAR2(13);
```

<br/>

#### 질의 3-38) NewBook 테이블의 isbn 속성의 데이터 타입을 NUMBER형으로 변경하시오.

```sql
ALTER TABLE NewBook MODIFY isbn NUMBER;
```

<br/>

#### 질의 3-39) NewBook 테이블의 isbn 속성을 삭제하시오.

```sql
ALTER TABLE NewBook DROP COLUMN isbn;
```

<br/>

#### 질의 3-40) NewBook 테이블의 bookid 속성에 NOT NULL 제약조건을 적용하시오.

```sql
ALTER TABLE NewBook MODIFY bookid NUMBER NOT NULL;
```

<br/>

#### 질의 3-41) NewBook 테이블의 bookid 속성을 기본키로 변경하시오.

```sql
ALTER TABLE NewBook ADD PRIMARY KEY(bookid);
```

<br/>

### DROP 문

* DROP 문은 테이블을 삭제하는 명령.
* DROP 문은 테이블의 구조와 데이터를 모두 삭제하므로 사용에 주의해야 함
* (데이터만 삭제하려면 DELETE 문을 사용함).

<br/>

#### DROP문의 기본 문법

```sql
DROP TABLE 테이블이름
```

<br/>

#### 질의 3-42) NewBook 테이블을 삭제하시오.

```sql
DROP  TABLE  NewBook;
```

<br/>

#### 질의 3-43) NewCustomer 테이블을 삭제하시오. 만약 삭제가 거절된다면 원인을 파악하고 관련된 테이블을 같이 삭제하시오(NewOrders 테이블이 NewCustomer를 참조하고 있음).

```sql
DROP TABLE  NewOrders;
DROP TABLE  NewCustomer;
```

<br/>

<br/>

## 05 데이터 조작어 DML - 삽입, 수정, 삭제

<br/>

### INSERT 문

* INSERT 문은 테이블에 새로운 투플을 삽입하는 명령

<br/>

#### INSERT 문의 기본 문법

```sql
INSERT  INTO 테이블이름[(속성리스트)]  // ★ INSERT INTO _ VALUES _
		VALUES (값리스트);
```

<br/>

#### 질의 3-44) Book 테이블에 새로운 도서 ‘스포츠 의학’을 삽입하시오. 스포츠 의학은 한솔의학서적에서 출간했으며 가격은 90,000원이다.

```sql
INSERT  INTO Book(bookid, bookname, publisher, price)
		VALUES (11, '스포츠 의학', '한솔의학서적', 90000);
```

* 속성을 같은 순서로 모두 넣을 경우, 속성의 이름은 생략해도 된다.
* 하지만 순서가 바뀌거나 일부 속성만 넣을 경우에는 반드시 명시해야 한다.
* 키워드 매핑 이라고 한다.

<br/>

#### 질의 3-45) Book 테이블에 새로운 도서 ‘스포츠 의학’을 삽입하시오. 스포츠 의학은 한솔의학서적에서 출간했으며 가격은 미정이다.

```sql
INSERT	INTO Book(bookid, bookname, publisher)
		VALUES (14, '스포츠 의학', '한솔의학서적');
```

<br/>

#### 대량삽입 (bulk insert)

* 한꺼번에 여러 개의 투플을 삽입하는 방법을 말한다.

<br/>

#### 질의 3-46) 수입도서 목록(Imported_book)을 Book 테이블에 모두 삽입하시오.

* (Imported_book 테이블은 스크립트 Book 테이블과 같이 이미 만들어져 있음) 

```sql
INSERT	INTO Book(bookid, bookname, price, publisher)
		SELECT bookid, bookname, price, publisher
		FROM  Imported_book;
```

<br/>

### UPDATE 문

* UPDATE 문은 특정 속성 값을 수정하는 명령이다.

<br/>

#### UPDATE 문의 기본 문법

```sql
UPDATE	테이블이름			// ★ UPDATE _ SET
SET		속성이름1=값1[, 속성이름2=값2, ...]
[WHERE <검색조건>];
```

<br/>

#### 질의 3-47) Customer 테이블에서 고객번호가 5인 고객의 주소를 ‘대한민국 부산’으로 변경하시오.

```sql
UPDATE 	Customer
SET 	address='대한민국 부산'
WHERE 	custid=5;
```

<br/>

#### 질의 3-48) Customer 테이블에서 박세리 고객의 주소를 김연아 고객의 주소로 변경하시오.

```sql
UPDATE	Customer
SET 	address = (	SELECT address
					FROM Customer
					WHERE name='김연아')
WHERE 	name LIKE '박세리';
```

<br/>

### DELETE 문

* DELETE 문은 테이블에 있는 기존 투플을 삭제하는 명령

<br/>

#### DELETE 문의 기본 문법

```sql
DELETE FROM    테이블이름		// ★ DELETE FROM _
[WHERE  검색조건];
```

<br/>

#### 질의 3-49) Customer 테이블에서 고객번호가 5인 고객을 삭제하시오.

```sql
DELETE 	FROM 	Customer
WHERE 	custid=5;
```

<br/>

#### 질의 3-50) 모든 고객을 삭제하시오.

```sql
DELETE 	FROM 	Customer;
```

<br/>