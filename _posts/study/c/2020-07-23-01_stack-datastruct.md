# Data Struct

<br/>

## 01. 스택 (Stack)

가장 기본이 되는 자료구조 중 하나이다. 자료를 표현하고 처리하는 방법에 관한 것이다.

스택의 경우 FILO 구조 (First In, Last Out) 이다. 가장 나중에 들어온 데이터가 먼저 처리된다.

스택과 큐는 반대되는 개념이다.

<br/>

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/2/29/Data_stack.svg/1280px-Data_stack.svg.png)

[출처 : 위키]

* LIFO (Last In First Out)라고도 불린다.
* 입구 및 출구가 하나 밖에 없는 경우이다.
* Push로 삽입, Pop 으로 삭제를 수행한다.

<br/>

### Stack 사용 예제 Source code

```c++
#include <iostream>
#include <stack>

using namespace std;

int main(void) {
	stack<int> s;
	s.push(7);
	s.push(5);
	s.push(4);
	s.pop();
	s.push(6);
	s.pop();
	while(!s.empty()) {
		cout << s.top() << ' ';
		s.pop();
	}
	return 0;
}
```

<br/>

[목록 보기](../README.md)