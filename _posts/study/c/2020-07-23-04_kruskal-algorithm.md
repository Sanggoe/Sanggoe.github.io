# Graph Algorithm

<br/>

## 04. 크루스칼 알고리즘 (Kruskal Algorithm)

* **가장 적은 비용**으로 모든 노드를 연결하기 위해 (최소 비용 신장트리) 사용하는 알고리즘이다.
* 즉, 최소 비용으로 단순히 모든 노드를 연결해서, 하나의 그래프로 연결되도록 만들어가는 알고리즘이라고 할 수 있다.
* 예를 들어, 모든 도시들을 도로로 연결할 때 가장 비용이 적게드는 방법을 찾을 때 사용한다.

<br/>

#### 용어 정리

* 노드 = 정점 = 도시 (그래프의 각 점을 말함)
* 간선 = 거리 = 비용 (그래프의 각 선분을 말함)

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfNzAg/MDAxNTIxMjcyNDE3NTE1.Ki-KVLqicCbFoCCRLoPttAp9uywjN4Wr3dbQ8pDWUVcg.GkpaLLCxBKH87gQ-LTpUvkrHXlITKMgqlXATPzQghRYg.PNG.ndb796/image.png?type=w773)

* 위 그래프를 예로 들면, 노드의 개수는 7개, 간선의 개수는 11개이다.
* 최소비용 신장트리를 만들 때, 소요되는 **간선의 숫자**는, 노드의 숫자 -1개 이다.
* 따라서 위의 경우 노드는 7개고, 필요한 간선은 6개면 된다.

<br/>

#### 크루스칼 알고리즘의 핵심

* **간선의 거리가 짧은 순서대로 그래프에 포함** 시키는 방법이다.
* 여기서 관심 있는 것은 모든 노드를 **최소 비용**으로 '**연결**' 하는데 목적이 있다.
* 따라서 간선 정보를 **오름차순으로 정렬**한 후 비용이 적은 간선부터 차례로 그래프에 포함시키면 된다. 

<br/>

#### 주의할 점

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMjUg/MDAxNTIxMjcxNzExNDc2.JOJ0EFAo1BuvQ02x0wMdUjjozrlHgsTXVB_g_J3Mhbgg.KjjBFTfWIbvSmMDm5XRaiQbOlUX8i-VHFzH4DwKI830g.PNG.ndb796/image.png?type=w773)

* 최소비용 신장트리를 만들 때 중요한 점은, 절대 **사이클이 발생하면 안된다**.
* 사이클이란 말 그대로 그래프가 서로 연결되어 사이클을 형성하는 경우를 말한다.
* 따라서 반드시 확인한 후 간선을 추가해야 한다.

<br/>

#### 노드 및 간선 정보 정렬

![img](https://postfiles.pstatic.net/MjAxODAzMTdfNjMg/MDAxNTIxMjcxNDAwMzI2.a7P0TK413KJxj7ekvhxVBBxhn3xUuOiN0tiWMgXhztog.rJmCIrBcC3F6pthX9VPbaqsVjzBeZw6r-XC9bGA0QhQg.PNG.ndb796/image.png?type=w773)

* 각 노드와 연결된 간선들의 정보를 위 형태 (연결된 두 노드의 번호, 간선의 거리) 로 저장한다.
* 위의 경우 노드 6과 7은 존재하지 않는다. 그 이유는, 이미 다른 노드와 연결된 간선에 포함되었기 때문이다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMTg3/MDAxNTIxMjcxNTI5NzM3.p2ITvXHnBQgWIC_fmSaAnwmZ4CbKzQO1eAFL7p7YXygg.t3lg0NLEaGdpZ8RyLf9q6_AictXPhR9SRZG2yn0C3asg.PNG.ndb796/image.png?type=w773)

* 위와 같이 모든 간선을 거리(비용)를 기준으로 정렬 한다.
* 아래의 알고리즘을 수행함으로서 최소 비용 신장 트리가 만들어진다.

<br/>

#### 알고리즘 과정

* 정렬된 순서에 맞게 비용이 작은 간선부터 그래프에 포함시킨다.
* 간선을 포함하기 전에 사이클 테이블을 확인한다.
* 만약 사이클을 형성하는 경우, 간선을 그래프에 포함시키지 않는다.
  * 사이클인지 판단하는 것은 [Union-Find 알고리즘](03_union-find.md)을 적용한다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMzcg/MDAxNTIxMjcxODkzNDA2.pYcapWvmsw6bTfAwGeEaU9To8t0m0Uk6mg-N9Kqws80g.9UbS7VxD5houNo4-ygtyG_5E1t5lRzAJ-NoDm_Cf370g.PNG.ndb796/image.png?type=w773)

* 각 노드와 간선들을 정렬하고, 사이클 테이블도 각 노드가 자신을 가리키는 상태의 초기 상태이다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMjgz/MDAxNTIxMjcyNTI3OTk4.hpVCLeefT8GT1Bf4ktRbWeUdICs5PRLoOkqEAj9UCskg.WDv-odAFO7Z7bloj8H8RgkImkpzB87lIWmJFsMNuAZIg.PNG.ndb796/image.png?type=w773)

* 첫 번째 간선을 선택해 연결한다.
* 사이클 테이블에서는 1과 7이 연결되었으므로 7의 부모노드를 1로 수정한다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMzAg/MDAxNTIxMjcyNTYzOTQx.xuD_xtC7f3xpOoEy-MJVYTXh-OqC4AEjzUJAGQgh54wg.y9DC8XAr0HJcJfseGNNh4MqTfA7dqgPvrMdkn2CqMFYg.PNG.ndb796/image.png?type=w773)

* 두 번째 간선을 선택하여 과정을 반복한다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMjE2/MDAxNTIxMjcyNjk5ODQx.ELST0EDElJpH_f2P4CFnYVwCI2e5XQz1XI0qphYCnuUg.9P8jgJ1S1T2O3n7_YAr7gnQA7L5n33zH4CSZ-gmJNHMg.PNG.ndb796/image.png?type=w773)

* 세 번째, 네 번째, 다섯 번째 간선을 선택한다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMTEw/MDAxNTIxMjcyNzE5Nzc1.1IfdyzZC6UAitlGk6ilwdQaBbQnmAHZx4AqTLvWfYXkg.vo2UjwY8ITHGwkGTm0p32G91P47wv1hZOm9f_8CQOPQg.PNG.ndb796/image.png?type=w773)

* 여섯 번째 간선의 경우 1과 4가 이미 연결되어 있으므로, 사이클이 발생할 수 있기 때문에 간선을 무시하고 넘어간다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMTIz/MDAxNTIxMjcyOTA4ODk0.PG3Bc_wVA48BSzis39faIOBvjeOPe5Pp69lEWUbQKUwg.hN5P05rlc1zdQW_nCs9hdfACL1ERmVgFkjuDReQxOWMg.PNG.ndb796/image.png?type=w773)

* 이후 일곱 번째 간선까지 추가하면, 최소비용 신장트리가 완성된다.
* 이후 다른 간선들도 마찬가지로 추가하지 않으므로 알고리즘 수행이 완료된다.

<br/>

### Source Code

```c
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

// 부모 노드를 가져옴 
int getParent(int set[], int x) {
	if(set[x] == x) return x;
	return set[x] = getParent(set, set[x]);
} 

// 부모 노드를 병합 
void unionParent(int set[], int a, int b) {
	a = getParent(set, a);
	b = getParent(set, b);
	// 더 숫자가 작은 부모로 병합
	if(a < b) set[b] = a;
	else set[a] = b;
} 

// 같은 부모를 가지는지 확인
int find(int set[], int a, int b) {
	a = getParent(set, a);
	b = getParent(set, b);
	if(a == b) return 1;
	else return 0;
}
 
// 간선 클래스 선언 
class Edge {
public:
	int node[2];
	int distance;
	Edge(int a, int b, int distance) {
		this->node[0] = a;
		this->node[1] = b;
		this->distance = distance;
	}
	bool operator <(Edge &edge) {
		return this->distance < edge.distance;
	}
};

int main(void) {
	int n = 7;
	int m = 11;
	
	vector<Edge> v;
	v.push_back(Edge(1, 7, 12));
	v.push_back(Edge(1, 4, 28));
	v.push_back(Edge(1, 2, 67));
	v.push_back(Edge(1, 5, 17));
	v.push_back(Edge(2, 4, 24));
	v.push_back(Edge(2, 5, 62));
	v.push_back(Edge(3, 5, 20));
	v.push_back(Edge(3, 6, 37));
	v.push_back(Edge(4, 7, 13));
	v.push_back(Edge(5, 6, 45));
	v.push_back(Edge(5, 7, 73));
	
	// 간선의 비용으로 오름차순 정렬 
	sort(v.begin(), v.end());
	
	// 각 정점이 포함된 그래프가 어디인지 저장 
	int set[n];
	for(int i = 0; i < n; i++) {
		set[i] = i;
	}
	
	// 거리의 합을 0으로 초기화 
	int sum = 0;
	for(int i = 0; i < v.size(); i++) {
		// 동일한 부모를 가르키지 않는 경우, 즉 사이클이 발생하지 않을 때만 선택 
		if(!find(set, v[i].node[0] - 1, v[i].node[1] - 1)) {
			sum += v[i].distance; 
			unionParent(set, v[i].node[0] - 1, v[i].node[1] - 1);
		}
	}
	
	printf("%d\n", sum);
}
```

<br/>

* 크루스칼 알고리즘은, 알고리즘 대회 문제에도 자주 출제되는 문제이다.
* **가장 적은 비용으로 모든 정점을 연결하는 방법**을 찾는 알고리즘이었다.
  * **최소비용 신장트리 생성 알고리즘**

<br/>

[모든 자료 및 사진 출처](https://blog.naver.com/PostView.nhn?blogId=ndb796&logNo=221230994142&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView)

[목록 보기](../README.md)