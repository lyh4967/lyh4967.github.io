---
title: "[Datastructure] 연결구조체 PLUS"
date: 2018-04-20 11:30:00
categories:
- Datastructure
tags:
- LINKED
---
**연결 구조체 PLUS**

전에 살펴봤던 연결리스트는 한가지 문제가 있다. 뒤따르는 노드의 액세스는 가능하지만 그에 앞서는 노드는 접근할 수 없다는게 그것이다. 그러나 선형리스트를 약간 바꾸면(마지막 노드의 `next`멤버에 있는 포인터가 NULL을 가지는대신 첫 번째 노드로 되돌아 가리키도록 하는 것) 단점을 해결할 수 있다. 이를 원형연결리스트라 칭한다. 또한 외부포인터를 리스트의 첫노드가 아닌 마지막노드를 가리키도록 한다면 첫노드와 마지막노드에 쉽게 접근할 수 있다.
<br/>
# CircularLinkedList
```c++
class CircularList{

  void FindItem(NodeType* listData, ItemType item, NodeType*& location, NodeType*& predLoc, bool& found){

    bool moreToSearch=true;
    location=listData->next;
    predLoc=listData;
    found=false;

    while(moreToSearch && !found){
      if(item < location->info)
        moreToSearch=false;
      else if(item==location->info)
        found=true;
      else{
        predLoc=location;
        location=location->next;
        moreToSearch=(location!=listData->next);
      }
    }
  }

  void InsertItem(ItemType item){
    Node* newNode;
    Node* predLoc;
    Node* location;

    bool found;

    newNode=new Node;
    newNode->info=item;
    if(listData!=NULL){
      FindItem(listData, item, location, predLoc, found);
      newNode->next=location;
      predLoc->next=newNode;

      if(listData==location){//삽입위치가 마지막노드일때
        listData=newNode;
      }
    }else{
      listData=newNode;
      newNode->next=newNode;
    }
    length++;
  }

  void DeleteItem(ItemType item){
    Node* location;
    Node* predLoc;
    bool found=true;

    FindItem(listData,item,location,predLoc,found);
    if(predLoc==location)//NULL
      listData=NULL;
    else{
      predLoc->next = location->next;
      if(location==listData)//가장 큰값삭제시
        listData=predLoc;
    }

    delete location;
    length--;
  }
}
```
사실 단일 원형리스트는 크게 이점을 얻지 못한다. 단일리스트와 다를 것이없기 때문이다. 원형리스트는 이중연결리스트를 응용하는데 도움을 준다. 이중 연결리스트는 노드의 양방향에 접근이 가능하다.

# DoubleLinkedList
```c++
class Node{
  ItemType info;
  Node* next;
  Node* prev;
}
```

검색 메서드를 사용시 자벌레 기법을 사용할 필요없이 뒤로 접근이 가능하다
```c++
class DoubleLinkedList{
}
void FindItem(Node* listData,ItemType item, Node*& location, bool&found){
  bool moreToSearch=true;

  location=listData;
  found=false;
  while(moreToSearch&&!found){
    if(item < locaton->info)
      moreToSearch=false;
    else if(item==location->info)
      found=true;
    else{
      location=location->next;
      moreToSearch=(location!=NULL);
    }
  }
}
```

# CopyConstructor
```c++
class stack{
  stack(const stack& another){
    NodeType* location = another.topPtr;
    NodeType* ptr;

    topPtr = new NodeType;
    topPtr->info = location->info;
    ptr = topPtr;
    location = location->next;
    while (location != NULL){
      NodeType* newNode = new NodeType;
      newNode->info = location->info;
      ptr->next = newNode;
      ptr = newNode;
      location = location->next;
    }
    ptr->next = NULL;
  }

stack operator=(const stack& another){
    NodeType* location = another.topPtr;
    NodeType* ptr;

    topPtr = new NodeType;
    topPtr->info = location->info;
    ptr = topPtr;
    location = location->next;
    while (location != NULL){
      NodeType* newNode = new NodeType;
      newNode->info = location->info;
      ptr->next = newNode;
      ptr = newNode;
      location = location->next;
    }
    ptr->next = NULL;
  }
}
```
기존의 insert연산과는 달리 앞에서 부터 뒤로추가해 나간다. 양방향리스트의 경우 location을 another의 제일 끝에 옮긴후 앞으로 움직이며 복사해 나갈수 있겠지만 스택은 그렇지 못하다. 따라서 앞에서 뒤로 요소를 추가해 나가며 가장 마지막의 노드의 `next`를 NULL로 지정하고 연산을 마무리한다.
