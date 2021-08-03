# Chap 04. SQL 고급

<br/>

<br/>

## 01 내장함수

### SQL 내장 함수

* SQL에서는 함수의 개념을 사용하는데 수학의 함수와 마찬가지로 특정 값이나 열의 값을 입력받아 그 값을 계산하여 결과 값을 돌려줌.
* SQL의 함수
  * DBMS가 제공하는 내장 함수(built-in function)
  * 사용자가 필요에 따라 직접 만드는 사용자 정의 함수(user-defined function)
* SQL 내장 함수는 상수나 속성 이름을 입력 값으로 받아 단일 값을 결과로 반환함.
* 모든 내장 함수는 최초에 선언될 때 유효한 입력 값을 받아야 함.

<br/>

#### 오라클에서 제공하는 주요 내장 함수

![image-20200519170123252](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200519170123252.png)

<br/>

#### 숫자 함수의 종류 

![image-20200519170237717](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200519170237717.png)

<br/>

* SYSDATE 는 많이 쓴다~ 

<br/>

### NULL 값 처리

#### NULL 값이란?

* 아직 지정되지 않은 값
* NULL 값은 ‘0’, ‘’ (빈 문자), ‘ ’ (공백) 등과 다른 특별한 값
* NULL 값은 비교 연산자로 비교가 불가능함.
* NULL 값의 연산을 수행하면 결과 역시 NULL 값으로 반환됨.

<br/>

#### 집계 함수를 사용할 때 주의할 점

* ‘NULL+숫자’ 연산의 결과는 NULL
* 집계 함수 계산 시 NULL이 포함된 행은 집계에서 빠짐
* 해당되는 행이 하나도 없을 경우 SUM, AVG 함수의 결과는 NULL이 되며,	COUNT 함수의 결과는 0.

<br/>

### ROWNUM

<br/>

```

```

<br/>

<br/>

## 02 부속질의 

<br/>

### 1. 스칼라 부속질의 - SELECT 부속질의

<br/>

#### 질의 4-12) 마당서점의 고객별 판매액을 보이시오(결과는 고객이름과 고객별 판매액을 출력).

```sql
SELECT	  ( SELECT name
          	FROM Customer cs
			WHERE cs.custid = od.custid ) "name", SUM(saleprice) "total"
FROM		Orders od
GROUP BY	od.custid;
```

<br/>

#### 질의 4-13) Orders 테이블에 각 주문에 맞는 도서이름을 입력하시오.

```sql
UPDATE 	Orders
SET 	bookname = ( SELECT bookname
					 FROM Book
					 WHERE Book.bookid=Orders.bookid );
```

* 테이블에 column 추가 

<br/>

### 2. 인라인 뷰- FROM 부속질의

#### 인라인 뷰(inline view)란?

* FROM 절에서 사용되는 부속질의
* 테이블 이름 대신 인라인 뷰 부속질의를 사용하면 보통의 테이블과 같은 형태로 사용할 수 있음
* 부속질의 결과 반환되는 데이터는 다중 행, 다중 열이어도 상관없음
* 다만 가상의 테이블인 뷰 형태로 제공되어 상관 부속질의로 사용될 수는 없음

<br/>

#### 질의 4-14) 고객번호가 2 이하인 고객의 판매액을 보이시오 (결과는 고객이름과 고객별 판매액 출력)

```sql
SELECT 	cs.name, SUM(od.saleprice) "total"
FROM 	(SELECT custid, name
	 	 FROM  Customer
		 WHERE custid <= 2) cs,
	 	 Orders od
WHERE 	 cs.custid=od.custid
GROUP BY cs.name;
--------------------------------------------------------------

* 위는 인라인 뷰 형태로 표현한 것 뿐, 아래와 같이 조인 해도 상관 없다.

SELECT 	 cs.name, SUM(od.saleprice) "total"
FROM 	 Customer cs, Orders od
WHERE 	 rownum <= 2 and cs.custid=od.custid
GROUP BY cs.name;
```

<br/>

![image-20200526135838788](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200526135838788.png)

<br/>

### 3. 중첩질의 – WHERE 부속질의

* 중첩질의(nested subquery)는 WHERE 절에서 사용되는 부속질의
* WHERE 절은 보통 데이터를 선택하는 조건 혹은 술어(predicate)와 같이 사용됨
* 그래서 중첩질의를 술어 부속질의(predicate subquery)라고도 함
* predicate : T / F

<br/>

![image-20200526140048057](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200526140048057.png)

<br/>

#### 비교 연산자

* 부속질의가 반드시 단일 행, 단일 열을 반환해야 하며, 아닐 경우 질의를 처리할 수 없음

#### 질의 4-15) 평균 주문금액 이하의 주문에 대해서 주문번호와 금액을 보이시오.

```sql
SELECT 	orderid, saleprice
FROM 	Orders
WHERE 	saleprice <= (SELECT AVG(saleprice)
                      FROM Orders);
```

* sub query에서 단일 값을 반환하고 있어 비교연산이 가능하다.

<br/>

#### 질의 4-16) 각 고객의 평균 주문금액보다 큰 금액의 주문 내역에 대해서 주문번호, 고객번호, 금액을 보이시오.

```sql
SELECT 	orderid, custid, saleprice
FROM 	Orders od
WHERE 	saleprice > (SELECT AVG(saleprice)
					 FROM Orders so
					 WHERE od.custid=so.custid);
--------------------------------------------------------------

* 위 SQL문을 아래와 같이 조인해도 상관 없다.
SELECT 	orderid, custid, saleprice
FROM 	Orders od, Orders so
WHERE 	saleprice > (SELECT AVG(saleprice)
					 FROM Orders so) and od.custid=so.custid;
```

* 고객별 평균 주문금액
* 부속질의로 들어간 부분은 부속질의를 없애고서도 여러가지 방법으로 가능하다~

<br/>

#### IN, NOT IN

*  IN 연산자는 주질의 속성 값이 부속질의에서 제공한 결과 집합에 있는지 확인(check)하는 역할을 함
* IN 연산자는 부속질의의 결과 다중 행을 가질 수 있음
* 주질의는 WHERE 절에 사용되는 속성 값을 부속질의의 결과 집합과 비교해 하나라도 있으면 참이 된다
* NOT IN은 이와 반대로 값이 존재하지 않으면 참이 됨

<br/>

#### 질의 4-17) 대한민국에 거주하는 고객에게 판매한 도서의 총판매액을 구하시오.

```sql
SELECT 	SUM(saleprice) "total"
FROM 	Orders
WHERE 	custid IN (SELECT custid
				   FROM Customer
				   WHERE address LIKE '%대한민국%');
				   
// 질의 4-19와 같은 질의!
```

* Orders와 Customer 테이블을 보며 확인해 보면, custid는 각각의 번호 1~5 이고, 부속질의에 해당하는 집합은 {2, 3, 5} 가 된다. (다중 행을 갖는 것이며, 허용 된다)

<br/>

#### ALL, SOME(ANY) 

* ALL은 모두, SOME(ANY)은 어떠한(최소한 하나라도)이라는 의미를 가짐
* 구문 구조
  * scalar_expression {비교연산자(=┃< >┃!=┃>┃>=┃!>┃<┃<=┃!<)}
    	          {ALL┃SOME┃ANY} (부속질의)

<br/>

#### 질의 4-18) 3번 고객이 주문한 도서의 최고 금액보다 더 비싼 도서를 구입한 주문의 주문번호와 금액을 보이시오.

```sql
SELECT 	orderid, saleprice
FROM 	Orders
WHERE 	saleprice > ALL (SELECT  saleprice
						 FROM   Orders
						 WHERE custid='3');
```

* \> ALL 인 경우 MAX이고, \> SOME이나 \> ANY 인 경우 MIN이다.

<br/>

#### EXISTS, NOT EXISTS

* 데이터의 존재 유무를 확인하는 **단항** 연산자
* 주질의에서 부속질의로 제공된 속성의 값을 가지고 부속질의에 조건을 만족하여 값이 존재하면 참이 되고, 주질의는 해당 행의 데이터를 출력함
* NOT EXIST의 경우 이와 반대로 동작함
* 구문 구조
  * WHERE [NOT] EXISTS (부속질의)	

<br/>

#### 질의 4-19) EXISTS 연산자로 대한민국에 거주하는 고객에게 판매한 도서의 총 판매액을 구하시오.

```sql
SELECT 	SUM(saleprice) "total"
FROM 	Orders od
WHERE 	EXISTS (SELECT *
				FROM Customer cs
				WHERE address LIKE '%대한민국%' AND cs.custid=od.custid);
				
// 질의 4-17과 같은 질의!
```

* 상관관계가 필수

<br/>

<br/>

## 03 뷰

<br/>

* 뷰(view)는 하나 이상의 테이블을 합하여 만든 가상의 테이블
* 장점 // ★
  * **편리성 및 재사용성**  : 자주 사용되는 복잡한 질의를 뷰로 미리 정의해 놓을 수 있음
    * 복잡한 질의를 간단히 작성
  * **보안성** : 각 사용자별로 필요한 데이터만 선별하여 보여줄 수 있음. 중요한 질의의 경우 질의 내용을 암호화 할 수 있음
    * 개인정보(주민번호)나 급여, 건강 같은 민감한 정보를 제외한 테이블을 만들어 사용 
  * **독립성 제공** : 미리 정의된 뷰를 일반 테이블처럼 사용할 수 있기 때문에 편리함. 또 사용자가 필요한 정보만 요구에 맞게 가공하여 뷰로 만들어 쓸 수 있음.
    * 원본 테이블이 구조가 변하여도 응용에 영향을 주지않도록하는 논리적 독립성 제공  

<br/>

* (뷰의 특징) // ★

1. **원본 데이터 값에 따라 같이 변함**
2. **독립적인 인덱스 생성이 어려움**
3. **삽입, 삭제, 갱신 연산에 많은 제약이 따름**

<br/>

### 뷰의 생성

* 기본 문법

```sql
CREATE VIEW 뷰이름 [(열이름 [ ,...n ])] // ★
AS SELECT 문
```

<br/>

* Book 테이블에서 ‘축구’라는 문구가 포함된 자료만 보여주는 뷰

```sql
SELECT	 *
FROM 	Book
WHERE 	bookname LIKE '%축구%';
```

<br/>

* 위 SELECT 문을 이용해 작성한 뷰 정의문

```sql
CREATE VIEW vw_Book
AS SELECT 	*
FROM 	    Book
WHERE 	    bookname LIKE '%축구%';
```

<br/>

#### 질의 4-20) 주소에 ‘대한민국’을 포함하는 고객들로 구성된 뷰를 만들고 조회하시오. 단, 뷰의 이름은 vw_Customer로 한다.

```sql
CREATE VIEW vw_Customer
AS SELECT     *
   FROM 	  Customer
   WHERE	  address LIKE '%대한민국%';
```

<br/>

#### 질의 4-21) Orders 테이블에 고객이름과 도서이름을 바로 확인할 수 있는 뷰를 생성한 후, ‘김연아’ 고객이 구입한 도서의 주문번호, 도서이름, 주문액을 보이시오.

```sql
CREATE VIEW vw_Orders (orderid, custid, name, bookid, bookname, saleprice, orderdate)
AS SELECT	od.orderid, od.custid, cs.name,
		    od.bookid, bk.bookname, od.saleprice, od.orderdate
     FROM	Orders od, Customer cs, Book bk
     WHERE	od.custid =cs.custid AND od.bookid =bk.bookid;
     
--------------------------------------------------------------

<결과 확인> 
SELECT 	orderid, bookname, saleprice
FROM 	vw_Orders
WHERE 	name='김연아';    
```

<br/>

### 뷰의 수정

* 기본 문법

```sql
CREATE OF REPLACE VIEW 뷰이름 [(열이름 [ ,...n ])]
AS SELECT 문
```

* **뷰를 이용해서 절대로 실제 튜플을 수정하거나 하지 않는다**.
* 그게 가능하면 엄청 복잡해진다...

<br/>

#### 질의 4-22) [질의 4-20]에서 생성한 뷰 vw_Customer는 주소가 대한민국인 고객을 보여준다. 이 뷰를 영국을 주소로 가진 고객으로 변경하시오. phone 속성은 필요 없으므로 포함시키지 마시오.

```sql
CREATE OR REPLACE VIEW vw_Customer (custid, name, address)
AS SELECT  custid, name, address
    FROM 	Customer
    WHERE 	address LIKE '%영국%';

--------------------------------------------------------------

<결과 확인> 
SELECT 	*
FROM 	vw_Customer;

<실제 수행>
SELECT 	*
FROM 	Customer
WHERE 	address LIKE '%영국%';
```

* **뷰는 query 형태로 저장이 된다. // ★**
* 위 <결과 확인> 코드의 vw_Customer는 실제로 <실제 수행> 코드로 치환되어 수행된다고 이해하자.
* 즉, **external schema와 conceptional schema 사이의 맵핑인 셈**이다.
* vw_Customer라는 것은, AS 이하 세 줄 코드이다 라는 맵핑

<br/>

#### 질의 4-23) 

```sql

```

<br/>

<br/>

## 04 인덱스
