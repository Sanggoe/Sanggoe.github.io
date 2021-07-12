# Graph Algorithm

<br/>

## 03. 합집합 찾기 (Union Find)

* 합집합 찾기라는 의미를 가진 **Union find** 알고리즘은, 대표적인 **그래프 알고리즘** 중 하나이다.
* 서로 같은 집합이 아닌 서로소 집합(disjoint-set)을 찾는 알고리즘이라고도 부른다.
* 여러 노드가 존재할 때 두 노드를 선택해서, **서로 같은 그래프에 속하는지 판별하는 알고리즘**이다.
* 나중에 강결합 컴포넌트(Strongly connected component) 라는 고급 알고리즘 등에서도 사용되는 개념이다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMTg4/MDAxNTIxMjY4NzgzMzA3.7GYrITyH43vDNEQyjobvZPnSZSxQrj80_SNf3Z1ma2Qg.jC6SGqtm9TcCi6gfXliOS5_Z-unifcv1jyVNE-zviwEg.PNG.ndb796/image.png?type=w773)

* 위와 같이 여러 노드가 서로 자유분방하게 존재한다고 가정한다.
* 즉, 다시 말해 모든 값이 자기 자신만을 집합의 원소로 가지고 있을 때를 말한다.

<br/>

|  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |

* 첫 행은 노드 번호를 의미
* 두 번째 행은 부모 노드 번호를 의미
* 처음에는 자기 자신만을 가리키고 있는 상태

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMTA1/MDAxNTIxMjY4ODEyMjQx.TtUYeJZUCi6P6Ujw--Rl30ZxZXX1n1ClTdkJZaMxFbAg.bpaiiYnA_QKetNd_-tPJ8TbthlTyouqzvrdamGlWkWAg.PNG.ndb796/image.png?type=w773)

* 이 때 1과 2가 연결되었다고 가정하자.
* 이러한 연결을 프로그래밍 언어로 어떻게 표현할 수 있을까? 하는 내용이 바로 Union-Find 라고 할 수 있다.

<br/>

|  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  1   |  1   |  3   |  4   |  5   |  6   |  7   |  8   |

* 2번째 노드의 부모 노드를 1로 설정하여 부모를 합친다.
* 일반적으로 부모를 합칠 때는 일반적으로 더 **작은 값** 쪽으로 합친다.
* 이것을 **합침(Union)** 이라고 한다.

<br/>

![img](https://postfiles.pstatic.net/MjAxODAzMTdfMjc2/MDAxNTIxMjY5MTA5NTY4.tjrnfM6w5TuZssambmJm37rfs50O-sEZuSZPtfQgQjEg.Qr6CJETHbQrxEZLHfayGb3tc0p5rf2ITWsLQnlP8OU4g.PNG.ndb796/image.png?type=w773)

* 이렇게 2와 3이 연결된 경우에는 어떻게 될까?

<br/>

|  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  1   |  1   |  2   |  4   |  5   |  6   |  7   |  8   |

* 위와 같이, 세 번째 노드의 부모를 2로 변경해준다.
* 위 표만 보고서는, 3이 1과 연결되었는지 알 수 없다.
* 따라서 **재귀 함수**가 사용된다.

<br/>

|  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  1   |  1   |  1   |  4   |  5   |  6   |  7   |  8   |

* 3이 2를 가리키고 있고, 2는 1을 가리키고 있으며 마지막 1은 자기 자신인 1을 가리키고 있다.
* 결과적으로 3 또한 1을 가리키고 있기 때문에, 3의 부모 노드의 값을 1로 수정해준다.
* 따라서 이 세 노드는 모두 같은 그래프에 속한다고 할 수 있다.
* **Find 알고리즘**은 이렇게, 두 개의 노드의 부모 노드를 확인하여 **현재 같은 집합에 속하는지 확인하는 알고리즘**이다.

<br/>

### Source Code

```c
#include <stdio.h>

int getParent(int parent[], int x) {
	if(parent[x] == x) return x;
	return parent[x] = getParent(parent, parent[x]);
}

// 각 부모 노드를 합칩니다. 
void unionParent(int parent[], int a, int b) {
	a = getParent(parent, a);
	b = getParent(parent, b);
	if(a < b) parent[b] = a;
	else parent[a] = b;
}

// 같은 부모 노드를 가지는지 확인합니다. 
int findParent(int parent[], int a, int b) {
	a = getParent(parent, a);
	b = getParent(parent, b);
	if(a == b) return 1;
	else return 0;
}

int main(void) {
	int parent[11];
	for(int i = 1; i <= 10; i++) {
		parent[i] = i;
	}
	unionParent(parent, 1, 2);
	unionParent(parent, 2, 3);
	unionParent(parent, 3, 4);
	unionParent(parent, 5, 6);
	unionParent(parent, 6, 7);
	unionParent(parent, 7, 8);
	printf("1과 5는 연결되어있나요? %d\n", findParent(parent, 1, 5));
	unionParent(parent, 2, 7);
	printf("1과 5는 연결되어있나요? %d\n", findParent(parent, 1, 5));
}
```

<br/>

#### 위 코드의 경우

![image-20200529200025055](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200529200025055.png)

* 처음에는 {1, 2, 3, 4}, {5, 6, 7, 8} 두 그래프로 나뉘어 있다.

<br/>

![image-20200529200502024](C:\Users\smpsm\AppData\Roaming\Typora\typora-user-images\image-20200529200502024.png)

<br/>

* 이 상태에서 위 코드는 2와 7을 unionParent 해준 뒤 1과 5가 같은 그래프에 있는지 확인했고, True를 반환한다.
* 즉, 다른 그래프에서 어떠한 노드이든지 상관 없이 그래프상에 연결 되어만 있다면 그것을 판단하는 알고리즘이다.
* {1, 2, 3, 4, 5, 6, 7, 8} 하나의 그래프에 모두 속하게 되었다.

<br/>

[모든 자료 및 사진 출처](https://blog.naver.com/PostView.nhn?blogId=ndb796&logNo=221230967614&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView)

[목록 보기](../README.md)