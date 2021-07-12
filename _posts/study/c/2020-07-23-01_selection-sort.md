# Sort Algorithm

<br/>

## 01. 선택정렬 (Selection Sort)

가장 기초적이며, 원시적인 방법 중 하나이며, 직관적이다.

<br/>

오름차순으로 정렬하는 경우에

**가장 작은 것을 선택해서 제일 앞으로 보내면 어떨까?**

하는 질문에서 출발하는 알고리즘이다.

<br/>

### Source code

------

```c
#include <stdio.h>

int main(void) {
	int i, j, temp;
	int array[10] = {1,10,5,8,7,6,4,3,2,9};
	
	for(i=0; i<9; i++) {
		j=i;
		while (array[j] > array[j+1]) {
			temp = array[j];
			array[j] = array[j+1];
			array[j+1] = temp;
			j--;
		}
	}
	
	for(i=0; i<10; i++) {
		printf("%d ", array[i]);
	}
	return 0; 
}
```



### 시간 복잡도

------

선택 정렬의 시간 복잡도는 : **N x (N + 1) / 2 => O(N^2)**

<br/>

[목록 보기](../README.md)