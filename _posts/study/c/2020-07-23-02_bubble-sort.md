# Sort Algorithm

<br/>

## 02. 버블 정렬 (Bubble sort)

마찬가지로, 매우 직관적인 해결 방법 중 하나이다.

숫자들을 오름차순으로 정렬하는 경우에서

**옆에 있는 값과 비교해서 더 작은 값을 앞으로 보내는 방법**이다.

가장 느리고 비효율적인 정렬 알고리즘이기도 하다.

<br/>

### Source code

------

```c
#include <stdio.h>

int main(void) {
	int i, j, temp;
	int array[10] = {1,10,5,8,7,6,4,3,2,9};

	for(i=0; i<10; i++) {
		for(j=0; j <9-i; j++) {
			if(array[j] > array[j+1]) {
				temp = array[j];
				array[j] = array[j+1];
				array[j+1] = temp;
			}
		}
	}
	for(i=0; i<10; i++) {
		printf("%d ", array[i]);
	}
	
	return 0; 
}
```

<br/>

### 시간 복잡도

------

버블 정렬의 시간 복잡도는 : **N x (N + 1) / 2 => O(N^2)**

<br/>

[목록 보기](../README.md)

