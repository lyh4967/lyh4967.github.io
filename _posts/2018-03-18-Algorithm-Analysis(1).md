---
title: "[Algorithm] 교환정렬, 합병정렬"
date: 2018-03-18 11:30:00
categories:
- Algorithm
tags:
- Algorithm
---
**교환정렬, 합병정렬**

n개의 데이타 (키값은 1~1000 사이의 자연수)를 정렬하는 문제

1. n개의 데이타를 random으로 생성

2. O(n^2) 인 exchange sort 알고리즘(A)와 O(n log n) 알고리즘인 merge sort 알고리즘(B)를 구현

3. 특정 n 에 대해 알고리즘 A, B 가 종료될 때까지의 시간을 측정한다.

4. A, B 를 1분간 수행할 때 해결할 수 있는 문제의 크기 n' 를 (2)의 결과를 이용하여 추정한다

5. n=3,000만 일 때의 A, B의 수행시간을  (2)의 결과를 이용하여 추정한다

# 교환정렬(exchange sort)
정렬되지 않은배열에서 선택된 인덱스의 키와 나머지 키를 비교하여 자신보다 낮은 경우 계속해서 교환하여 가장 작은 키를 인덱스 순차적으로 위치시키는 알고리즘.

예를 들어 1부터 9까지 아홉개의 숫자가 무작위로 섞여있는 배열이 있는 경우 0번째 인덱스에 있는 키를 선택한 후, 나머지 1부터 8까지 인덱스에 있는 키와 모두 비교하여 최소값인 1이 맨 앞에 오게 한다. 1번째 인덱스에도 마찬가지로 나머지 2부터 8까지 인덱스에 있는 키와 모두 비교하여 최소값 2가 오게 한다.

c++코드
```c++
void exchangeSort(int n, int arr[]){
	clock_t begin, end;
	begin = clock();
	for (int i = 0; i < n-1; i++)
	for (int j = i + 1; j < n; j++)
	if (arr[i] < arr[j]){
		int tmp = arr[i];
		arr[i] = arr[j];
		arr[j] = tmp;
	}

	end = clock();
	cout << "수행시간 : " << (end - begin) << endl;
}
```

# 합병정렬
합병 정렬은 분할정복 방식으로 설계된 알고리즘이다.
<br/>

입력으로하나의 배열을 받고, 연산 중에 두개의 배열로 계속 조개 나간뒤, 합치면서 정렬해 최후에는 하나의 정렬을 출력한다.

**분할 과정의 기본로직**
1. 현재 배열을 반으로 쪼갠다. 배열의 시작 위치와, 종료 위치를 입력받아 둘을 더한 후 를 나눠 그 위치를 기준으로 나눈다.
2. 이를 쪼갠 배열의 크기가 0이거나1일때까지 반복한다.
<br/>

**합병 과정의 기본로직**
1. 두 배열 A,B의 크기를 비교한다. 각각의 배열의 현재 인덱스를 i,j로 가정한다.
2. i에는 A배열의 시작 인덱스를 저장하고, j에는 B배열의 시작 주소를 저장한다.
3. A[i]와 B[j]를  비교한다. 오름차순의 경우 이중에 작은 값을 새 배열 C에 저장한다.
4. 이를i나j 둘중 하나가 각자 배열의 끝에 도달할 때까지 반복한다.
5. 끝까지 저장을 못한 배열의 값을, 순서대로 전부 다 C에 저장한다.
6. C배열을 원래의 배열에 저장해준다.

c++코드
```c++
void merge(int start, int end, int arr[],int tmp[]){
	int mid = (start + end) / 2;//첫번째 배열의 끝 인덱스

	int i = start;//첫번째 배열의 시작인덱스
	int j = mid + 1;//두번째 배열의 시작인덱스

	int k = start;
	while (i <= mid&&j <= end){//두 배열의 끝에 이르기까지반복
		if (arr[i] >=  arr[j]){
			tmp[k] = arr[i];//임시배열에 첫번째 배열 원소추가
			i++;
		}
		else{
			tmp[k] = arr[j];
			j++;
		}
		k++;
	}
	//두 배열을 비교하고 남은원소들을  임시배열에 추가
	int t;//남은 배열의 시작인덱스
	if (i > mid)
		t = j;
	else
		t = i;
	for (k; k <= end; k++, t++)
		tmp[k] = arr[t];
	for (k = 0; k <= end; k++)
		arr[k] = tmp[k];
}

void mergeSort(int start, int end, int arr[], int tmp[]){

	int mid;
	if (start < end){
		mid = (start + end) / 2; //배열의 중간인덱스
		mergeSort(start, mid, arr,tmp);//왼쪽배열분할
		mergeSort(mid + 1, end, arr,tmp);//오른쪽배열 분할
		merge(start, end, arr,tmp);
	}
}
```
