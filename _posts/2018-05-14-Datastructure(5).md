---
title: "[Datastructure] 이진 검색 트리"
date: 2018-05-14 11:30:00
categories:
- Datastructure
tags:
- BST
---
**이진 검색 트리**

**이진트리의 정의: 단일 시작 노드를 가진 자료 구조로 각 노드는 두 개의 자식 노드를 가진다. 그리고 루트로부터 트리 내의 모든 노드로는 유일한 경로가 존재한다.**

# 논리적 레벨

* 각 노드는 자식이라고 불리는 두 개의 상속노드를 가질 수 있다. 또한 이진 트리에 있는 각 자식 노드는 역시 두개의 자식노드를 가질 수 있고 계속해서 이렇게 이루어져서 트리의 가지 구조를 이룬다. 여기서 시작점은 루트라고 불리는 단일 시작노드이다. 여기서 트리의 노드가 자식노드를 가지지 않으면 리프라 부른다.

* 노드의 레벨은 루트로부터 떨어진 거리를 의미한다. 또한 트리 내의 최대 레벨은 그것의 높이가 된다. 어떠한 N레벨을 가진 노드의최대 수는 2^N이다. 또한 최소 레벨 수는 주어진 노드들을 다 사용할때 까지 모든 노드에 두개의 자식노드를 할당하면 logN+1의 레벨을 가진다.

**왼쪽 서브트리에 있는 노드의 키들은 그 서브트리의 키보다 작고 오른쪽 서브트리에 있는 노드의 키들은 그 서브트리의 키보다 크다.**

# 구현 레벨

```c++
bool TreeType::IsFull() const
// Returns true if there is no room for another item
//  on the free store; false otherwise.
{
  TreeNode* location;
  try
  {
    location = new TreeNode;
    delete location;
    return false;
  }
  catch(std::bad_alloc exception)
  {
    return true;
  }
}

bool TreeType::IsEmpty() const
// Returns true if the tree is empty; false otherwise.
{
  return root == NULL;
}

int CountNodes(TreeNode* tree);

int TreeType::LengthIs() const
// Calls recursive function CountNodes to count the
// nodes in the tree.
{
  return CountNodes(root);
}


int CountNodes(TreeNode* tree)
// Post: returns the number of nodes in the tree.
{
  if (tree == NULL)
    return 0;
  else
    return CountNodes(tree->left) + CountNodes(tree->right) + 1;
}

void Retrieve(TreeNode* tree,
     ItemType& item, bool& found);

void TreeType::RetrieveItem(ItemType& item, bool& found)
// Calls recursive function Retrieve to search the tree for item.
{
  Retrieve(root, item, found);
}


void Retrieve(TreeNode* tree,
     ItemType& item, bool& found)
// Recursively searches tree for item.
// Post: If there is an element someItem whose key matches item's,
//       found is true and item is set to a copy of someItem;
//       otherwise found is false and item is unchanged.
{
  if (tree == NULL)
    found = false;                     // item is not found.
  else if (item < tree->info)      
    Retrieve(tree->left, item, found); // Search left subtree.
  else if (item > tree->info)
    Retrieve(tree->right, item, found);// Search right subtree.
  else
  {
    item = tree->info;                 // item is found.
    found = true;
   }
}

void Insert(TreeNode*& tree, ItemType item);

void TreeType::InsertItem(ItemType item)
// Calls recursive function Insert to insert item into tree.
{
  Insert(root, item);
}


void Insert(TreeNode*& tree, ItemType item)
// Inserts item into tree.
// Post:  item is in tree; search property is maintained.
{
  if (tree == NULL)
  {// Insertion place found.
    tree = new TreeNode;
    tree->right = NULL;
    tree->left = NULL;
    tree->info = item;
  }
  else if (item < tree->info)
    Insert(tree->left, item);    // Insert in left subtree.
  else
    Insert(tree->right, item);   // Insert in right subtree.
}
void DeleteNode(TreeNode*& tree);

void Delete(TreeNode*& tree, ItemType item);

void TreeType::DeleteItem(ItemType item)
// Calls recursive function Delete to delete item from tree.
{
  Delete(root, item);
}


void Delete(TreeNode*& tree, ItemType item)
// Deletes item from tree.
// Post:  item is not in tree.
{
  if (item < tree->info)
    Delete(tree->left, item);   // Look in left subtree.
  else if (item > tree->info)
    Delete(tree->right, item);  // Look in right subtree.
  else
    DeleteNode(tree);           // Node found; call DeleteNode.
}   

void GetPredecessor(TreeNode* tree, ItemType& data);

void DeleteNode(TreeNode*& tree)
// Deletes the node pointed to by tree.
// Post: The user's data in the node pointed to by tree is no
//       longer in the tree.  If tree is a leaf node or has only
//       non-NULL child pointer the node pointed to by tree is
//       deleted; otherwise, the user's data is replaced by its
//       logical predecessor and the predecessor's node is deleted.
{
  ItemType data;
  TreeNode* tempPtr;

  tempPtr = tree;
  if (tree->left == NULL)
  {
    tree = tree->right;
    delete tempPtr;
  }
  else if (tree->right == NULL)
  {
    tree = tree->left;
    delete tempPtr;
  }
  else
  {
    GetPredecessor(tree->left, data);
    tree->info = data;
    Delete(tree->left, data);  // Delete predecessor node.
  }
}

void GetPredecessor(TreeNode* tree, ItemType& data)
// Sets data to the info member of the right-most node in tree.
{
  while (tree->right != NULL)
    tree = tree->right;
  data = tree->info;
}

void PrintTree(TreeNode* tree, std::ofstream& outFile)
// Prints info member of items in tree in sorted order on outFile.
{
  if (tree != NULL)
  {
    PrintTree(tree->left, outFile);   // Print left subtree.
    outFile << tree->info;
    PrintTree(tree->right, outFile);  // Print right subtree.
  }
}

void TreeType::Print(std::ofstream& outFile) const
// Calls recursive function Print to print items in the tree.
{
  PrintTree(root, outFile);
}

TreeType::TreeType()
{
  root = NULL;
}

void Destroy(TreeNode*& tree);

TreeType::~TreeType()
// Calls recursive function Destroy to destroy the tree.
{
  Destroy(root);
}


void Destroy(TreeNode*& tree)
// Post: tree is empty; nodes have been deallocated.
{
  if (tree != NULL)
  {
    Destroy(tree->left);
    Destroy(tree->right);
    delete tree;
  }
}

void TreeType::MakeEmpty()
{
  Destroy(root);
  root = NULL;
}


void CopyTree(TreeNode*& copy,
     const TreeNode* originalTree);

TreeType::TreeType(const TreeType& originalTree)
// Calls recursive function CopyTree to copy originalTree
//  into root.
{
  CopyTree(root, originalTree.root);
}

void TreeType::operator=
     (const TreeType& originalTree)
// Calls recursive function CopyTree to copy originalTree
// into root.
{
  {
  if (&originalTree == this)
    return;             // Ignore assigning self to self
  Destroy(root);      // Deallocate existing tree nodes
  CopyTree(root, originalTree.root);
  }

}
void CopyTree(TreeNode*& copy,
     const TreeNode* originalTree)
// Post: copy is the root of a tree that is a duplicate
//       of originalTree.
{
  if (originalTree == NULL)
    copy = NULL;
  else
  {
    copy = new TreeNode;
    copy->info = originalTree->info;
    CopyTree(copy->left, originalTree->left);
    CopyTree(copy->right, originalTree->right);
  }
}
TreeNode* TreeType::PtrToSuccessor_recur(TreeNode*&  tree) {//재귀
	if (tree->left != NULL)
		PtrToSuccessor_recur(tree->left);
	else {
		TreeNode* tempPtr = tree;
		tree = tree->right;
		return tempPtr;
	}
}
```
몇몇 연산들의 이름을 바꾸었지만 저번에 포스팅한 연결구조체의 함수와 비슷한다. 멤버 함수중 재귀구조를 사용하였다. 재귀구조를 사용할때 의 이점은 반복문을 사용할때에 비해서 문맥파악이 용이하다. 또한 매개변수에 레퍼런스 연산을 하여 추가적인 노드의 포인터값 변환없이 변수를 참조하여 자동으로 수정된 포인터를 가리키도록 할 수 있다. 이 부분이 이해가 안간다면 댓글로 달아주길 바란다.

# PrintTree
이번 절에선 트리를 순회하는 3가지 방법에 대해 알아보고자 한다.<br/>
이진 트리를 순회하기 위해선 트리의 루트 노드를 가리키도록 포인터를 초기화 한다.
1. 중위순회(inorder traversal)
먼저 로트 노드의 왼쪽 서브트리노드를 방문하고 루트노드를 방문한다. 그 후 오른쪽 서브트리의 노드를 방문한다.
```c++
void InOrder(TreeNode* tree,
     QueType& inQue)
// Post: inQue contains the tree items in inorder.
{
  if (tree != NULL)
  {
    InOrder(tree->left, inQue);
    inQue.Enqueue(tree->info);
    InOrder(tree->right, inQue);
  }
}
```
2. 후위순회(postorder traversal)
한 노드의 왼쪽 서브트리노드를 방문하고 다음에 오른쪽 서브트리 노드를 방문한 후 루트 노드를 방문한다
```c++
void PostOrder(TreeNode* tree,
     QueType& postQue)
// Post: postQue contains the tree items in postorder.
{
  if (tree != NULL)
  {
    PostOrder(tree->left, postQue);
    PostOrder(tree->right, postQue);
    postQue.Enqueue(tree->info);
  }
}
```
3. 전위순회(preorder traversal)
지정된 노드를 먼저 방문하고 다음에 그 노드의 왼쪽 서브트리 노드를 방문한다. 마지막으로 오르쪽 서브트리노드를 방문한다.
```c++
void PreOrder(TreeNode* tree,
     QueType& preQue)
// Post: preQue contains the tree items in preorder.
{
  if (tree != NULL)
  {
    preQue.Enqueue(tree->info);
    PreOrder(tree->left, preQue);
    PreOrder(tree->right, preQue);
  }
}
```
# Big-O 비교

| /|이진검색트리|배열기반리스트|연결리스트|
|:--|:----------|:-----------|:---------|
|생성자|O(1)|O(1)|O(1)|
|소멸자|O(N)|O(1)|O(N)|
|IsFull|O(1)|O(1)|O(1)|
|IsEmpty|O(1)|O(1)|O(1)|
|Retrieve| | | |
|검색|O(logN)|O(logN)|O(N)|
|처리|O(1)|O(1)|O(1)|
|전체|O(logN)|O(logN)|O(N)|
|InsertItem| | | |
|검색|O(logN)|O(N)|O(N)|
|처리|O(1)|O(N)|O(N)|
|전체|O(logN)|O(1)|O(N)|
|DeleteItem| | | |
|검색|O(logN)|O(logN)|O(N)|
|처리|O(1)|O(N)|O(1)|
|전체|O(logN)|O(N)|O(N)|

> 배열기반 리스트의 항목드이 포인터를 가진다면 소멸자의 경우 다시 할당되어져야 하므로 O(N)이 된다.
