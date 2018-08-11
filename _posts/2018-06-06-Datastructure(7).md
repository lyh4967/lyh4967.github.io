---
title: "[Datastructure] Heap Sort, 힙 정렬 알고리즘"
date: 2018-06-06 11:30:00
categories:
- Datastructure
tags:
- HEAP
- SORT
---
**여러 정렬 알고리즘에 대해 알아봅시다**

# Heap Sort Pre
이 자료구조의 장점은 다음에 선택할 최대원소를 어디에서 찾아야 할지 알고 있다는 점에서 이점이 있다.
1. 힙의 루트원소를 꺼내고 적절한 위치에 넣는다.
2. 남아있는 원소를 다시 힙시킨다.
3. 더 이상 남은 원소가 없어질 때까지 반복해서 수행한다.
힙의 특성에 따라 매번 O(log(N))번의 비교만 하면 된다.
<br/>

# Build Heap
`ReheapDown`연산을 반복한다. 이 함수는 각 단계별로 모든 서브트리에 적용된다. 그 뒤 트리의 단계를 올려서 루트 노드에 이를때 까지 `ReheapDown`을 진행한다. `ReheapDown`이 루트 노드에 대해 호출되고 나면 전체 트리는 힙의 순서 속성을 만족시키게 된다.
```c++
// Convert array  values[0..numValues-1] into a heap
void BuildHeap(int index, int numValues, ItemType values[]){
	for (index = numValues / 2 - 1; index >= 0; index--)
		ReheapDown(values, index, numValues - 1);
  }

template<class ItemType>
void ReheapDown(ItemType elements[], int root, int bottom)
// Post: Heap property is restored.
{
	int maxChild;
	int rightChild;
	int leftChild;

	leftChild = root * 2 + 1;
	rightChild = root * 2 + 2;
	if (leftChild <= bottom)
	{
		if (leftChild == bottom)
			maxChild = leftChild;
		else
		{
			if (elements[leftChild] <= elements[rightChild])
				maxChild = rightChild;
			else
				maxChild = leftChild;
		}
		if (elements[root] < elements[maxChild])
		{
			Swap(elements[root], elements[maxChild]);
			ReheapDown(maxChild, bottom);
		}
	}
}
```
힙을 만드는 과정은 비단말(nonleaf) 노드를 루트노드로 `ReheapDown`연산하는 것으로 시작한다.
![build-heap](https://user-images.githubusercontent.com/33022147/41023763-42ca4476-69a8-11e8-93b1-9f130a00c5df.PNG)
위 그림의 첫번째 비단말 노드는 8 이며 배열의 인데스로 따졌을때 `numValues/2-1`이다. 차례차례 이를 이용해 13 4 7 6 순서로 전체 트리의 루트노드에 도달할때 까지 연산을 진행해 heap를 만든다.
# Heap Sorting
이제부터 정렬을 시작한다.
```c++
void ExecuteHeapSort(int index, int numValues, ItemType values[])
for (index = numValues - 1; index >= 1; index--)
	{
		Swap(values[0], values[index]);
		ReheapDown(values, 0, index - 1);
	}
```
힙의 최대값은 `values[0]`로  배열엔 `values[numvalues-1]`의 요소로 들어가 이 둘을 바꾼다. 이때 힙의 구조가 깨질것이기 때문에 `ReheapDown`으로 힙의 구조를 유지시킨다. 이 과정은 모든 원소가 적합한 곳에 위치할 때까지 반복하여 수행된다.

# Analysis of HeapSort
루프는 N-1번 동안 원소를 맞바꾸고 다시 힙정렬을 하면서 수행하게된다. 그리고 비교연산은 `ReheapDown`함수에서 일어난다. N개의 노드를 갖고 있는 완전 이진 트리는 O(log(N+1))의 깊이를 갖는다. 따라서 `ReheapDown`은 O(log(N))이 된다. 이런 수행을 N-1번 반복하므로 결국 O(Nlog(N))의 복잡도를 갖게된다. N이 작을경우 오버헤드로 좋은성능을 얻지는 못하지만 N이 매우커질수록 좋은 성능을 갖게된다.
