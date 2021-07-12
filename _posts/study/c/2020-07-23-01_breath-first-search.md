# Graph Algorithm

<br/>

## 01. 너비 우선 탐색 (BFS)

* 탐색할 때 **너비를 우선**으로 하여 탐색을 수행하는 탐색 알고리즘
* 시작점과 가까운 것 위주로 탐색하는 기법
* 맹목적인 탐색 하고자 할 때 사용하는 방법
* **Quere**를 사용해 알고리즘이 수행된다.

<br/>

(아래 '=' 사이에 노드가 들어가는 큐를 의미한다.)

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTZfMTA3/MDAxNTIxMTgwNDAzNjA0.UHWvRTbVmw6QVDorhYZgSBpDAUI90FWNRj2SVraQL-8g.dlSTfXYCJEtptG0p71HQckj8i56_LnCIhtGkC_cxvYgg.PNG.ndb796/image.png?type=w773)

* BFS는 시작 노드(Start Node)를 큐에 삽입하며 시작한다.
* 삽입한 노드들은 '방문 처리' 해준다. (분홍색으로 표시)

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTZfMjgx/MDAxNTIxMTgxNTE5NzA1.5ui24F9iF_0jLLkda_oI4dlJSQeJCsUX-NaltK7zl1og.00nI0SnR1F8XezyXKUcR13lzPMEMEWjUc75mhfrH1aQg.PNG.ndb796/image.png?type=w773)

#### BFS는 다음과 같은 알고리즘에 따라 작동한다.

* 큐에서 하나의 노드를 꺼낸다
* 해당 노드에 연결된 노드 중 방문하지 않은 노드를 방문하고, 차례대로 큐에 삽입한다.
* 위 과정을 반복수행한다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTZfMjA3/MDAxNTIxMTgxOTIzNzY2.HNHjTAyjqxA_i2jejRMwpyhZdD6jkZj81X_fZ8UL4AMg.rmqPi6prf0g81341bFGgPC4FqYwHnfZasCjbo6T16MYg.PNG.ndb796/image.png?type=w773)

* 큐에서 꺼낸 Start 노드 1과 인접한 노드인 2와 3을 방문한 뒤 큐에 삽입한다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTZfODUg/MDAxNTIxMTgyMDk0NzA1.TJNwLI8Y5Yi3RTbFOwtko5rHDkyhROG2fJimEeVM2Oog.E4y_RrvVid7BO5FkdyjTUQw3Cai-Kjh07QvnP7FJb6cg.PNG.ndb796/image.png?type=w773)

* 큐에서 꺼낸 노드인 2와 인접한 노드 4, 5를 방문하고 큐에 삽입한다.

<br/>![img](https://postfiles.pstatic.net/MjAxODAzMTZfMjAy/MDAxNTIxMTgyMTkzMDg3.iYTA1bQOvfWZlMlfT7zGxsaC2Ulqjndgop8RtBCHOVEg.D1qrG99pyf4vYlPkLbyiN5Hs8z55TuQXqdMBMe0cYw4g.PNG.ndb796/image.png?type=w773)

* 큐에서 꺼낸 노드인 3과 인접한 노드 6, 7을 방문하고 큐에 삽입한다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTZfMjc1/MDAxNTIxMTgyMjc0ODc0.rsN_4H-xqNQls_-LYV8oBwa2S6svKfT-0RSfY7a8K0Yg.fAkxep6hFhh8sSgxrYErVYkvQzSz5CVMdwMJaZXuRGAg.PNG.ndb796/image.png?type=w773)

* 큐에서 다음 노드를 꺼내고, 더이상 방문할 노드가 존재하지 않으므로 차례로 큐에서 노드를 꺼낸다.

<br/>

### Source Code

```c
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

int number = 7;
int c[7];
vector<int> a[8];

void bfs(int start) {
	queue<int> q;
	q.push(start);
	c[start] = true;
	while(!q.empty()) {
		int x = q.front();
		q.pop();
		printf("%d ", x);
		for(int i = 0; i < a[x].size(); i++) {
			int y = a[x][i];
			if(!c[y]) {
				q.push(y);
				c[y] = true;
			}
		}
	}
}

int main(void) {
	// 1과 2을 연결합니다. 
	a[1].push_back(2);
	a[2].push_back(1);
	// 1과 3를 연결합니다.
	a[1].push_back(3);
	a[3].push_back(1);
	// 2과 3를 연결합니다.
	a[2].push_back(3);
	a[3].push_back(2);
	// 2과 4을 연결합니다. 
	a[2].push_back(4);
	a[4].push_back(2);
	// 2과 5를 연결합니다.
	a[2].push_back(5);
	a[5].push_back(2);
	// 3와 6를 연결합니다.
	a[3].push_back(6);
	a[6].push_back(3);
	// 3와 7을 연결합니다.
	a[3].push_back(7);
	a[7].push_back(3);
	// 4와 5를 연결합니다.
	a[4].push_back(5);
	a[5].push_back(4);
	// 6과 7을 연결합니다.
	a[6].push_back(7);
	a[7].push_back(6); 
	// BFS를 수행합니다.
	bfs(1); 
	return 0;
}
```

<br/>

## BFS

* 너비우선 탐색의 방법이 다른 알고리즘에 적용이 되어 효과적으로 사용이 된다는 것 자체에 의미가 있다.
* BFS는 하나의 탐색 방법이기 때문에 이 자체는 큰 의미가 없다.
* 나중에 응용을 위해 개념을 배운 것!

<br/>

[모든 자료 및 사진 출처](https://blog.naver.com/PostView.nhn?blogId=ndb796&logNo=221230944971&proxyReferer=https:%2F%2Fwww.google.com%2F)

[목록 보기](../README.md)