---
title: "[Datastructure] 연결구조체"
date: 2018-04-20 11:30:00
categories:
- Datastructure
tags:
- LINKED
---
**연결 구조체**

하나의 배열에서 스택을 구현하기는 간단하지만 중대한 결점이있다. 스택 객체를 선얼할 때 스택의 크기를 결정해야하는것이다. 이는 상당한 메모리낭비를 야기시킨다. 하지만 요소를 한번에 하나씩 동적할당을 하면 위의 단점을 보완할 수 있다.<br/>

new연산자는 type값을 수용할 정도크기의 메모리블록을 할당하고 그 블록의 주소를 리턴시킨다. 이를 이용해 각각의 요소를 연결시키고 그 각각을 노드라 칭한다. 기본적으로 노드에는 갖고있는 value와 노드와 연결되어있는 `next`노드 포인터를 포함한다. 그럼 스택과 큐, 리스트를 이용해 연결 구조체를 구현해보도록 한다.<br/>

 ```c
 struct Node{
	 ItemType info;
	 Node* next;
 }
 ```
 요약하자면 stack는 오직 하나의 데이터멤버를 가지는 클래스이다. 스택에서 최상부 노드를 가리키는 포인터이다.
# LinkedStack
 ```c
 class stack{
	void Push(ItemType newItem){
		if(this->IsFull()){
			throw PushOnFullStack();
		}else{
			NodeType<ItemType>* location;
			location=new NodeType<ItemType>;
			location->info=newItem;
			location->next=topPtr;
			topPtr=location;
		}
	}
	void Pop(ItemType& item){
		if(IsEmptr())
			throw PopOnEmptyStack();
		NodeType<ItemType>* tempPtr;
		item=topPtr->info;
		tempPtr=topPtr;
		topPtr=topPtr->next;
		delete tempPtr;
	}

	bool IsFull() const{
		NodeType<ItemType>* location;
		try{
			location=new NodeType<ItemType>;
			delete location;
			return false;
		}
		catch(bac_alloc exception){//메모리 할당 실패
			return true;
		}
	}

	void MakeEmpty(){
		NodeType<ItemType>* tempPtr;
		while(topPtr!=NULL){
			tempPtr=topPtr;
			delete tempPtr;
			topPtr=topPtr->next;
		}
	}
}
 ```
 해당자리에 하나의노드를 삭제하거나추가하고 기존의 순서를 유지하기위해 전 노드를 앞의 노드에 연결시켜준다. 처음 넣을시 topPtr의 값은 `NULL`이다. 또한 IsFull()은 힙에 메모리할당이 가능한지를 검사한다.<br/>
# LinkedQueue
 ```c
class queue{
	void MakeEmpty(){
		Node* tempPtr;
		while(front!=NULL){
			tempPtr=front;
			front=front->next;
			delete tempPtr;
		}
		rear=NULL;
	}

	void Enqueue(ItemType newItem){
		Node* newNode;
		newNode->info=newItem;
		newNode->next=NULL;
		if(rear==NULL){
			front=newNode;
		}else{
			rear->next=newNode;
		}
		rear=newNode;			
	}
	void Dequeue(ItemType& item){
		Node* tempPtr;
		item=front->info;
		tempPtr=front;
		front=front->next;
		if(front==NULL)
			rear=NULL;
		delete tempPtr;
	}
}
```
큐의 경우도 스택에서의 Node와 비슷하게 활용할 수 있다.
# Linked UnsortedList
```c
class UnsortedList{
	void DeleteItem(ItemType item){
		Node* location=listData;
		Node* tempLocation;
		if(item==listData->info){
			tempLocation=location;
			listData=listData->next;
		}else{
			while(!(item==(location->next)->info))
				location=location->next;
			tempLocation=location->next;
		}
		delete tempLocation;
	}
}
```
연결리스트에서 인자를 제거하는 함수구현의 키워드는 `next->info`를 비교하는 것이다. 현재 `location`의 `info`가 `item`과 같아도 그전의노드를 다음노드와 연결할 방법이 없기 때문이다.

# LinkedSortedList
```c
class SortedList{
	void InsertItem(ItemType item){
			Node* newNode;
			Node* predLoc;
			Node* location;
			bool moreToSearch;

			location=listdata;
			moreToSearch=(location!=NULL);

			while(moreToSearch){
				if(location->info<item){
					predLoc=location;
					location=location->next;
					moreToSearch = location!=NULL
				}else{
					moreToSearch=false;
				}
			}
			newNode=new Node;
			newNode->info=item;

			if(predLoc==NULL){
				newNode->next=listData;
				listData=newNode;
			}else{
				newNode->next=location;
				predLoc->next=newNode;
			}
			length++;
	}
}
```
정렬된 연결리스트에서 나머지 멤버함수들은 별다를 것없지만 `InsertItem`에서는 한가지 트릭들어간다. 정렬을 위해서 삽입위치를 검색해야하는데 위치를 찾았다면 그 저노드에 접근할 방법이 없다. 비정렬리스트의 DeleteItem에서 사용했던 방식으로 `location->next->info`의 사용을 고려해볼 수 있겠지만 이는 삽입위치가 마지막노드일 경우 NULL에 접근하게 되므로 에러를 발생시킨다. 따라서 `predLoc`와 `location` 두 포인터를 써서 조건을 만족한다면 해당 포인터의 사이에 노드를 삽입시키도록 한다.
