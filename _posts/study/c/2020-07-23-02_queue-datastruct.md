# Data Struct

<br/>

## 02. 큐 (Queue)

가장 기본이 되는 자료구조 중 하나이다. 자료를 표현하고 처리하는 방법에 관한 것이다.

큐의 경우 FIFO 구조 (First In, First Out) 이다. 가장 먼저 들어온 데이터가 먼저 처리된다.

스택과 큐는 반대되는 개념이다.

<br/>

![img](https://t1.daumcdn.net/cfile/tistory/996EE0395A523F7E0E)

[출처 : https://meylady.tistory.com/9]

* 입구와 출구가 각각 존재하는 경우이다.
* Push로 삽입, Pop 으로 삭제를 수행한다.

<br/>

### Queue 사용 예제 Source code

```c++
#include <iostream>
#include <queue>

using namespace std;

int main(void) {
	queue<int> q;
	q.push(7);
	q.push(5);
	q.push(4);
	q.pop();
	q.push(6);
	q.pop();
	while(!q.empty()) {
		cout << q.front() << ' ';
		q.pop();
	}
	return 0;
}
```

<br/>

[목록 보기](../README.md)