---
title: "[Datastructure] B-Tree를 알아보자"
date: 2018-05-22 11:30:00
categories:
- Datastructure
tags:
- B-TREE
---
**균형잡는 트리 Balanced Tree 인 B-tree에 대해 알아보자**

# Balanced Tree의 필요성

트리의 성능을 결정하는 것은 트리의 높이이다. 하지만 기존 BST에 최악의 경우  O(N)의 검색시간이 측정되는 이른반 선형구조가 형성될 수가 있다. 하지만 Balanced Tree는 트리가 변할때 필요하면 스스로 균형을 유지한다.
>ex) AVL Tree, 2-3(-4) Tree,Red-Black Tree, B-Tree ...

항상 O(logN)의 검색성능을 갖는다.

# B-Tree의 개요
B-tree는 하나의 노드에 여러자료가 배치되는 트리구조이다. 한 노드에 M개의 자료가 배치되면 M차 B-Tree이며 M이 짝수이냐 홀수이냐에 따라 알고리즘이 다르다.

# B-Tree 규칙 1
* 노드의 자료수가 n이라면, 자식의 수는 N+1이어야 한다.
* 각 노드의 자료는 정렬된 상태 여야한다.
* 노드의 자료 Dk의 왼쪽 서브트리는 Dk보다 작은 값들이고, Dk의 오른쪽 서브트리는 Dk보다 큰 값들이어야 한다.

# B-Tree 규칙 2
* 루트노드는 적어도 2개이상의 자식을 가져야 한다.
* 루트노드를 제외한모든노드는 적어도 M/2개의 자료를 가지고 있어야 한다.
* 외부노드로 가는 경로의 길이는 모두 같다.
 - 외부노드는 모두같은 레벨에 있다..
* 입력자료는 중복될 수 없다.

# B-Tree의 노드
```c++
template<class TYPE>
class Node {
public:
	TYPE* m_pKeys;
	Node** m_pChildren;//노드 포인터의 배열
	int m_nKeys;

	Node(int dim) {
		m_pKeys = new TYPE[dim];
		m_pChildren = new Node*[dim + 1];
		for (int i = 0; i <= dim; i++) m_pChildren[i] = 0;
		m_nKeys = 0;
	}
	~Node() {
		delete[] m_pKeys;
		delete[] m_m_pChildren;
	}
	Node<TYPE>*& Left(int n) { return m_pChildren[n]; }
	Node<TYPE>*& Right(int n) { return m_pChildren[n + 1]; }
	TYPE& Key(int n) { return m_pKeys[n]; }
	int& Size() { return m_pKeys[n]; }
	void AddValue(const TYPE& value, Node* left, Node* right);
	void DelValue(int index);
	int FindKey(const TYPE& key);
	void Clear(int dim);
};

template<class TYPE>
void Node<TYPE>::AddValue(const TYPE& value, Node* left, Node* right) {
	int i;
	i = m_nKeys;//삽입정렬
	while (i > 0 && Key(i - 1) > value) {
		Key(i) = Key(i - 1);
		Left(i + 1) = Left(i);
		i--;
	}
	m_nKeys++;
	Key(i) = value;
	Left(i) = left;
	Rigth(i) = right;
}

template<class TYPE>
void Node<TYPE>::DelValue(int index) {
	for (int i = index + 1; i < Size(); i++) {
		Key(i - 1) = Key(i);
		Left(i - 1) = Left(i);
	}
	Left(i - 1) = Left(i);
	m_nKeys--;
}

template<class TYPE>
int Node<TYPE>::FindKey(const TYPE& key) {
	for (int i = 0; i < m_nKeys; i++) {
		if (key == Key(i)) return i;
	}
	return -1;
}

template<class TYPE>
void Node<TYPE>::Clear(int dim) {
	m_nKeys = 0;
	for (int i = 0; i <= dim; i++) m_pChildren[i] = 0;
}

```

# B-Tree의 검색
```c++
template<class TYPE>
bool BTreeMap<TYPE>::Find(const TYPE& key, TYPE& value) const {
	Node<TYPE>* t;
	int index;
	t = m_pNodeHead->Left(0);//루트
	while (t != 0 && (index = t->FindKey(key) < 0)) {
		for (int i = 0; i<t->Size() && key>t->Key(i); i++)
			t = t->Left(i);
	}
	if (t == 0)return false; //못찾음

	value = t->Key(index);//찾음
	return true;
}
```

# B-Tree의 삽입
```c++
template<class TYPE>
Node<TYPE>* BTreeMap<TYPE>::_Split(const TYPE& key, Node<TYPE>* pivot) {
	Node<TYPE>* left;
	Node<TYPE>* right;
	Node<TYPE>* split;   // 리턴할 노드
	int i, j;

	right = new Node<TYPE>(m_nDim);

	// 경우 1 : 분할할 노드가 Root 인 경우
	if (pivot == m_pNodeHead)
	{
		split = pivot->Left(0);		// child == root

									// left child 생성
		left = new Node<TYPE>(m_nDim);
		for (i = 0; i < m_nDim / 2; i++)
		{
			left->Key(i) = split->Key(i);
			left->Left(i) = split->Left(i);
		}
		left->Left(i) = split->Left(i);
		left->Size() = m_nDim / 2;

		// right child 생성
		for (i = m_nDim / 2 + 1, j = 0; i < m_nDim; i++, j++)
		{
			right->Key(j) = split->Key(i);
			right->Left(j) = split->Left(i);
		}
		right->Left(j) = split->Left(i);
		right->Size() = m_nDim / 2;

		// 부모노드 정리
		TYPE temp = split->Key(m_nDim / 2);
		split->Clear(m_nDim);
		split->AddValue(temp, left, right);
	}
	// 경우 2 : 분할할 노드가 Root가 아닌 경우
	else
	{
		// 분할할 노드를 찾기
		for (i = 0; i < pivot->Size() && key > pivot->Key(i); i++);

		// 왼쪽 노드는 이미 있는 노드이므로 갯수만 조정
		left = pivot->Left(i);
		left->Size() = m_nDim / 2;

		// 오른쪽 노드 생성
		for (i = m_nDim / 2 + 1, j = 0; i < m_nDim; i++, j++)
		{
			right->Key(j) = left->Key(i);
			right->Left(j) = left->Left(i);
		}
		right->Left(j) = left->Left(i);
		right->Size() = m_nDim / 2;

		// 중간키를 부모에 삽입
		pivot->AddValue(left->Key(m_nDim / 2), left, right);
		split = pivot;
	}
	return split;
}

template<class TYPE>
bool BTreeMap<TYPE>::Insert(const TYPE& value) {
	Node<TYPE>* t,* p;
	int i;
	p = m_pNodeHead;
	t = m_pNodeHead->Left(0);
	if (t == 0)  // 뿌리노드가 없다면 특별히 생성해주어야 한다.
	{
		t = new Node(m_nDim);
		m_pNodeHead->Left(0) = t;
	}
	while (t != 0)
	{
		if (t->FindKey(value) >= 0)		// 중복키 삽입금지
			return false;
		if (t->Size() == m_nDim)	// 꽉찬 노드이면 분할한다.
			t = _Split(value, p);
		p = t;
		for (i = 0; i < t->Size() && value > t->Key(i); i++);
		t = t->Left(i);
	}
	p->AddValue(value, 0, 0);   // 외부노드로 삽입
	m_nCount++;
	return true;
}

```
삽입 알고리즘
- 자료는 항상 leaf노드에 추가
 * leaf노드의 선택은 삽입될키의 하향탐색에 의해 결정
- 추가될 leaf노드에 여유가 있다면 그냥 삽입
- 추가될 leaf노드에 여유가 없다면 '분할'하기
- 분할을 위해서는 키 하나를 부모로 올려야함
 * 부모가 꽉 차있다면 문제!
 * 삽입을 위한 하향탐색을 하면서 꽉찬노드는 분할해 주어야 함

# B-Tree의 삭제
```c++
template<class TYPE>
bool BTreeMap<TYPE>::_BorrowKey(Node<TYPE>* p, int index) {//p는 삭제 대상의 부모노드,index는 빌릴놈의 인덱스
	int from, to;
	Node<TYPE>* p1;
	Node<TYPE>* p2;
	to = index;
	if (index == p->Size())	// 가장 오른쪽인 경우 왼쪽형제에게 빌림
		from = index - 1;
	else  // 아니면 오른쪽 형제에게서 빌림
		from = index + 1;
	p1 = p->Left(from);   // p1 = 빌려주는 노드
	p2 = p->Left(to);       // p2 = 빌리는 노드
	if (p1->Size() <= m_nDim / 2)   // 빌려줄 키가 없을 때 실패를 리턴
		return false;
	if (from > to)   // 오른쪽 형제에게서 빌림
	{
		p2->AddValue(p->Key(to), p2->Left(p2->Size()), p1->Left(0));
		p->Key(to) = p1->Key(0);
		p1->DelValue(0);
	}
	else   // 왼쪽 형제에게서 빌림
	{
		p2->AddValue(p->Key(from), p1->Left(p1->Size()), p2->Left(0));
		p->Key(from) = p1->Key(p1->Size() - 1);
		p1->DelValue(p1->Size() - 1);
	}
	return true;
}

template<class TYPE>
Node<TYPE>* BTreeMap<TYPE>::_BindNode(Node<TYPE>* p, int index) {
	Node<TYPE>* left;
	Node<TYPE>* right;
	int i;
	if (index == p->Size()) index--;	// 가장 오른쪽이면 index 감소
	left = p->Left(index);
	right = p->Right(index);
	left->Key(left->Size()++) = p->Key(index);   // 왼쪽노드에 부모키를 복사
	for (i = 0; i < right->Size(); i++)  // 왼쪽노드에 오른쪽 노드를 복사
	{
		left->Key(left->Size()) = right->Key(i);
		left->Left(left->Size()++) = right->Left(i);
	}
	left->Left(left->Size()) = right->Left(i);
	p->DelValue(index);   // 부모노드에서 결합한 키를 삭제
	p->Left(index) = left;  // 포인터 조절
	delete right;
	if (p->Size() == 0)   // 뿌리노드일 수 밖에 없음 이경우....
	{
		delete p;
		m_pNodeHead->Left(0) = left;
	}
	return left;  // 결합된 노드를 리턴
}

template<class TYPE>
TYPE BTreeMap<TYPE>::_SwapKEy(Node<TYPE>* del, int index) {
	Node<TYPE>* cdd;
	Node<TYPE>* cddp;    // cdd는 대체노드, cddp는 대체노드의 부모
	cddp = del;
	cdd = cddp->Right(index);   // 삭제키의 오른쪽 자식
	while (cdd->Left(0) != 0)
	{
		cddp = cdd;
		cdd = cdd->Left(0);
	}
	del->Key(index) = cdd->Key(0);   // 키 대체
	return cdd->Key(0);
}

template<class TYPE>
bool BTreeMap<TYPE>::Remove(const TYPE& key) {
	Node<TYPE>* t;
	Node<TYPE>* p;
	int pi = 0;   // 부모의 index
	int ti = 0;   // 현재노드의 index
	TYPE value = key;

	p = m_pNodeHead;
	t = m_pNodeHead->Left(0);
	while (t != 0)
	{
		if (t->Size() <= m_nDim / 2 && p != m_pNodeHead) // 확장할 필요가 있으면 확장
		{
			if (!_BorrowKey(p, pi))		// 형제에게서 빌려보고 실패하면 형제와 결합
				t = _BindNode(p, pi);
		}

		if ((ti = t->FindKey(value)) >= 0)  // 삭제키가 이 노드에 있으면
		{
			if (t->Left(0) == 0) break;  // 외부노드이면 break;
			else value = _SwapKey(t, ti);   // 내부노드이면 바꿈. 이제 새로운 value를 아래로 내려야 한다.
		}
		p = t;
		for (pi = 0; pi < t->Size() && (value > t->Key(pi) || value == t->Key(pi)); pi++);
		t = t->Left(pi);
	}
	if (t == 0) return false;   // 찾을 수 없음.
	if (t->Size() <= m_nDim / 2 && p != m_pNodeHead)  // 외부노드인데 키수가 너무 적으면
		if (!_BorrowKey(p, pi)) t = _BindNode(p, pi);
	t->DelValue(t->FindKey(value));  // 노드의 키를 삭제
	m_nCount--;
	return true;
}
```
삭제 알고리즘
* 삭제하고자 하는 자료가 있는 노드가 삭제후 자료수가 M/2이상이 되도록 보장해야함
* 형제에게서 빌리기 vs 형제와 결합하기
- 빌리기: 형제가 M/2보다 많은 자료를 가지고 있차있다면-> `BorrowKey`
- 결합하기: 형제에게서 빌릴 수 없다-> `BindNode`
* 삭제키가 있는 노드가 내부노드인 경우
- 대체키를 찾아 대체해야 한다. Right Subtree중 가장 큰값으로!(이진트리와 동일)
* 삭제를 위한 하향탐색을 하면서 자료수가 M/2이하인 노드는 형제에게 빌리든지 결합해야함.

# B-Tree 분석
* M차 B-Tree의 높이는 logM/2(N) 이하이다.
- 각 노드는 최소 M/2개의 자료를 갖는다.
* 검색시 각 노드당 최대 M번의 순차검색
* 삽입/ 삭제/ 검색 성능
- T(N)< c*M*logM/2(N),즉 O(logN)의 성능
* B-Tree는 외부검색에 적합함
- 하나의 노드크기를 DiskI/O 단위의 크기로!
- 순차검색과 트리검색의 잇점을 취함
- B-Tree는 Balanced Tree로 최악의 경우가 없음
- 퀵정렬에서 소구간에 대한 삽입정렬이 성능이 좋았다.
- 노드내에서 순차검색 대신 이분검색은?

# 결론
1. Balanced Tree
- 스스로 균형을 유지하는 이진검색트리
- 최적의 검색 성능을 보장
2. B-Tree
- 한 노드에서 M개의 자료, M+1개의 자식을 가질수 있음
- 삽입 알고리즘
* 노드가 꽉찾을 경우 Split동작으로 노드 분할
- 삭제 알고리즘
* 노드의 자료수는 M/2 이상이어 하는 원칙에 의해 노드의 자료수가 적으면 형제에게서 빌리거나 형제와 결합
- 삽입/검색/삭제 모두 O(loN)의 성능을 보임
- 외부검색에도 효율적인 검색구조.

실제로 Oracle 혹은 MSSQL 서버에서 인덱스를 이용한 데이터저장에 B-Tree를 사용하고 있다.
