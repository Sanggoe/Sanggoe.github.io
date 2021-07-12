# Sort Algorithm

<br/>

## 06. C++ STL sort() 함수

정렬은 이미 오래된 연구 분야이므로, **<u>이미 훌륭한 정렬 관련 라이브러리가 존재</u>**한다.

다만 내부 정렬 원리를 모르고 무작적 쓰는 것은 좋은 방법이 아니다.

따라서 앞서 나온 여러 정렬 방법들을 잘 공부하고, 숙지하는 것은 선행되어야 할 것이다.

<br/>

## sort() 함수의 기본 사용법

algorithm 이라는 헤더파일에는 STL 라이브러리 sort 함수가 정의되어 있다.

**sort(first_address, last_address);**

매개변수로 정렬할 메모리의 첫 주소, 마지막 주소를 준다.

보통 (배열이름, 배열이름 + 길이) 로 준다.

<br/>

### Source code

```C++
#include <iostream>
#include <algorithm>

using namespace std;

int main(void) {
	int a[10] = {9,3,5,4,1,10,8,6,7,2};
	int i;
	sort(a, a+10); // 배열의 메모리주소, 마지막 주소
	for (i=0; i<10; i++) {
		printf("%d ", a[i]);
	} 
}
```

<br/>

 sort() 함수는 **<u>오름차순 정렬을 기본</u>**으로 수행한다.

이 함수의 장점 중 하나가, 이 정렬 기준을 사용자가 지정해 줄 수 있다는 점이다.

<br/>

```c++
bool compare(int a, int b) { // 작은 값이 앞으로 즉, 오름차순 
	return a < b;
}
```

<br/>

이렇게 비교 함수를 정의해주는 것이다. 참 거짓에 대한 값으로 정렬 기준을 판단한다는 의미이다.

위의 경우에는 a < b 라서 작은 값이 앞으로 즉, 오름차순으로 정렬이 된다.

<br/>

```c++
bool compare(int a, int b) { // 큰 값이 앞으로 즉, 내림차순 
	return a > b;
}
```

<br/>

만약 비교를 바꿔주게 되면 손쉽게 내림차순으로 변경할 수 있는 것이다.

compare를 활용한 전체 소스코드를 보면 다음과 같다.

<br/>

### Source code

```C++
#include <iostream>
#include <algorithm>

using namespace std;

bool compare(int a, int b) { // 큰 값이 앞으로 즉, 내림차순 
	return a > b;
}

int main(void) {
	int a[10] = {9,3,5,4,1,10,8,6,7,2};
	int i;
	sort(a, a+10, compare); // 배열의 메모리주소, 마지막 주소
	for (i=0; i<10; i++) {
		printf("%d ", a[i]);
	} 
}
```

<br/>

## 데이터를 묶어서 정렬하는 방법

앞서 배웠던 정렬들 예시의 경우처럼 단순 데이터 정렬 기법은 **실무에서 사용하기에는 적합하지 않다**.

실무에서는 데이터들이 객체로 정리되어 내부에 여러 변수들을 포함하고 있기 때문이다.

따라서, 이런 경우 중요한 정렬 방식은 **<u>특정한 변수를 기준으로</u>** 정렬 하는 것이다.

다음은 **Class**를 정의해서 여러 변수가 존재하는 상황에 대해 정렬하는 방법을 표현하였다.

<br/>

```C++
#include <iostream>
#include <algorithm>

using namespace std;

class Student {
	public:
		string name;
		int score;
		Student(string name, int score) {
			this->name = name;
			this->score = score;
		}
		
		// 정렬 기준은 '점수가 낮은 순서'
		bool operator <(Student &student) {
			return this->score < student.score;
		}
};

int main(void) {
	Student students[] = {
		Student("상 고 이 ", 90),
		Student("김 길 동 ", 93),
		Student("박 길 동 ", 97),
		Student("홍 길 동 ", 87),
		Student("최 길 동 ", 92)
	};
	
	sort(students, students + 5);
	
	for (int i=0; i<5; i++) {
		cout << students[i].name << ' ';
	} 
}
```

<br/>

조금 더 실무에 가까운, 현실적인 데이터로 정렬이 이루어지는 경우이다.

Student 라는 객체에서 score 값을 기준으로 정렬을 수행한 후, name 값을 출력하는 형태이다.

<br/>

출력 결과는 아래와 같다.

```c++
홍 길 동  상 고 이  최 길 동  김 길 동  박 길 동
--------------------------------
Process exited after 0.08552 seconds with return value 0
계속하려면 아무 키나 누르십시오 . . .
```

<br/>

다만, 클래스를 정의하는 방식은 속도 측면에서 별로 효율적이지 못하다. 

다시 말해 **<u>실무에서는 적합한 해결 방법</u>**이고, <u>**프로그래밍 대회에서는 적절한 방법이 아니다**</u>.

후자의 경우에는, **<u>페어(Pair) 라이브러리</u>**를 사용하는 것이 효율적이다.

<br/>

```c++
#include <iostream>
#include <vector> // c++ 에서는 요걸 자주 쓴다. 
#include <algorithm>

using namespace std;

int main(void) {
	vector<pair<int, string> > v; // 앞에서부터 first, second 정보를 가진다.
	v.push_back(pair<int, string>(90, "상고이 "));
	v.push_back(pair<int, string>(85, "삼등임 "));
	v.push_back(pair<int, string>(82, "사등임"));
	v.push_back(pair<int, string>(98, "일등임 "));
	v.push_back(pair<int, string>(79, "오등임 "));
	
	sort(v.begin(), v.end());
	
	for (int i=0; i<v.size(); i++) {
		cout << v[i].second << ' ';
	}
	
	return 0; 
}
```

<br/>

**벡터(Vector) STL**은, 배열보다 사용하기 쉽게 개편하여 삽입(Push)과 삭제(Pop)가 용이한 자료구조이다. 

**페어(Pair) STL**은, 한 쌍의 데이터를 처리할 수 있도록 해주는 자료구조이다.

이런 STL을 적절히 혼용하여 소스코드를 줄이는 기법을 **숏코딩(Short Coding)** 이라고 한다.

<br/>

## 두 개의 기준으로 정렬하는 방법

학생의 정보가 이름, 성적, 생년월일 일 경우, 학생을 성적순으로 나열하고 싶다.

다만, 동일한 성적의 경우 나이가 어린 순서로 나열한다.

위 요구사항에 대한 정렬 역시 벡터와 페어를 이용해 숏코딩 할 수 있다.

<br/>

< 학생 명단 >

학생 1 : 홍길동/90점/1996-12-13

학생 2 : 상고이/97점/1997-05-09

학생 3 : 김길동/95점/1994-07-23

학생 4 : 홍고이/97점/1995-03-29

학생 5 : 박고이/88점/1990-06-02

<br/>

기본적으로 성적 순으로 정렬은 한다.

만약 성적이 같을 경우에는, 생년월일을 기준으로 정렬한다.

<br/>

### Source code

```c++
#include <iostream>
#include <vector> // c++ 에서는 요걸 자주 쓴다. 
#include <algorithm>

using namespace std;

bool compare(pair<string, pair<int, int> > a, pair<string, pair<int, int> > b) {
	if(a.second.first == b.second.first) { // 성적이 같다면
		return a.second.second > b.second.second; // 생년월일이 느린(어린) 학생 먼저 
	} else {
		return a.second.first > b.second.first;
	} 
}

int main(void) {
	vector<pair<string, pair<int, int> > > v; // 앞에서부터 first, second 정보를 가진다.
	v.push_back(pair<string, pair<int, int> >("홍길동", pair<int, int>(90, 19961213)));
	v.push_back(pair<string, pair<int, int> >("상고이", pair<int, int>(97, 19970509)));
	v.push_back(pair<string, pair<int, int> >("김길동", pair<int, int>(90, 19940723)));
	v.push_back(pair<string, pair<int, int> >("홍고이", pair<int, int>(97, 19950329)));
	v.push_back(pair<string, pair<int, int> >("박고이", pair<int, int>(95, 19900602)));
	
	sort(v.begin(), v.end(), compare);
	
	for (int i=0; i<v.size(); i++) {
		cout << v[i].first << ' ';
	}
	
	return 0; 
}
```

```c++
상고이 홍고이 박고이 홍길동 김길동
--------------------------------
Process exited after 0.1659 seconds with return value 0
계속하려면 아무 키나 누르십시오 . . .
```

<br/>

위와 같이, 중첩 pair를 이용한다면 정렬 기준이 두 개인 경우에도 효율적으로 프로그래밍 할 수 있다.

단, 정렬 기준이 세 개가 넘어갈 경우에는 오히려 class를 정의하는 것이 더 효율적일 수도 있다.

**STL 라이브러리**를 얼마나 많이, 잘 아느냐가 알고리즘의 깊이를 결정할 수 있다!!

<br/>

[목록 보기](../README.md)