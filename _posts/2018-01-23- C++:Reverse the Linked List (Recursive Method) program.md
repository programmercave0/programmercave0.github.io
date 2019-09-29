---
layout: post
title: "C++:Reverse the Linked List (Recursive Method) program"
tags:  [C++, Algorithm, Linked List, Data Structures]
date: 2018-01-23
---

Here we are going to reverse a Linked List. We are going to use Recursive method to reverse a Linked List.

```
Given linked list - 2-->3-->4-->5-->6-->nullptr
After reversing -   6-->5-->4-->3-->2-->nullptr

Given linked list - p-->r-->o-->g-->r-->a-->m-->nullptr
After reversing -   m-->a-->r-->g-->o-->r-->p-->nullptr
```

Time Complexity : O(n)
Space Complexity : O(n)

```cpp
#include <iostream>
#include <utility>

template <class T>
class LinkedList
{
  struct Node
  {
    T data;
    Node * next;
    Node(T value) : data(std::move(value)), next(nullptr) {}
  };
  Node *head;

public:
  LinkedList() : head(nullptr) {}
  LinkedList(const LinkedList& ll) = delete; //copy constructor
  LinkedList(const LinkedList&& ll) = delete; //move constructor
  LinkedList& operator=(const LinkedList& ll) = delete; //copy assignment
  LinkedList& operator=(const LinkedList&& ll) = delete; //move assignment
  ~LinkedList();
  void insert(T);
  void printList();
  void recursiveReverse()
  {
    head = recursiveReverse(head);
  }
private:
  Node* recursiveReverse(Node* head)
  {
    if(head == nullptr)
      return nullptr;

    if(head->next == nullptr)
      return head;

    Node *firstElement = head;
    Node *secondElement = firstElement->next;
    head = firstElement->next;
    firstElement->next = nullptr; //unlink first node
    Node *remainingList = recursiveReverse(head);
    secondElement->next = firstElement;
    return remainingList;
  }
};

template <class T>
void LinkedList<T>::insert(T data)
{
  Node *node = new Node(std::move(data));
  Node *tmp = head;
  if(tmp == nullptr)
  {
    head = node;
  }
  else
  {
    while(tmp->next != nullptr)
    {
      tmp = tmp->next;
    }
    tmp->next = node;
  }
}

template <class T>
void LinkedList<T>::printList()
{
  Node *node = head;
  while(node)
  {
    std::cout << node->data << " ";
    node = node->next;
  }
  std::cout<<"\n";
}
template <class T>
LinkedList<T>::~LinkedList()
{
  Node *tmp = nullptr;
  while(head)
  {
    tmp = head;
    head = head->next;
    delete tmp;
  }
  head = nullptr;
}

int main()
{
  LinkedList<int> ll1;
  ll1.insert(2);
  ll1.insert(3);
  ll1.insert(4);
  ll1.insert(5);
  ll1.insert(6);
  ll1.printList();
  ll1.recursiveReverse();
  ll1.printList();
}
```

This Recursive Method is not suitable for longer Linked List because our function ```recursiveReverse``` places a pointer to the top list element on the stack and recursively call itself to place pointer to next element on the stack. If our linked list is longer than the space available to stack, our program will crash.

 <input type="hidden" name="IL_IN_ARTICLE"> 
 
You may also like:
[C++: Move all Even numbers before Odd numbers in Singly Linked List (Using STL)](https://programmercave.github.io/blog/2018/02/08/C++-Move-all-Even-numbers-before-Odd-numbers-in-Singly-Linked-List-(Using-STL))

[C++: Merge two sorted Linked List (in-place)](https://programmercave.github.io/blog/2018/02/06/C++-Merge-two-sorted-Linked-List-(in-place))

[C++: Split Singly Circular Linked List program](https://programmercave.github.io/blog/2018/02/04/C++-Split-Singly-Circular-Linked-List-program)

[C++: Doubly Circular Linked List program](https://programmercave.github.io/blog/2018/02/02/C++-Doubly-Circular-Linked-List-program)

[C++: Reverse the Linked List (Iterative Method) program](https://programmercave.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Iterative-Method)-program)

[C++: Linked List containing Loop (Floyd Cycle finding Algorithm) program](https://programmercave.github.io/blog/2018/01/20/C++-Linked-List-containing-Loop-(Floyd-Cycle-finding-Algorithm)-program)

[C++: Swapping Node Links in Linked List](https://programmercave.github.io/blog/2017/08/23/C++-Swapping-Node-Links-in-Linked-List)

[C++: Queue implementation using Linked List (Data Structure)](https://programmercave.github.io/blog/2017/08/01/C++-Queue-implementation-using-Linked-List-(Data-Structure))

[C++: Stack implementation using LinkedList (Data Structure)](https://programmercave.github.io/blog/2017/07/31/C++-Stack-implementation-using-LinkedList-(Data-Structure))

[C++: Doubly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/28/C++-Doubly-Linked-List-using-Template-(Data-Structure))

[C++: Singly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/27/C++-Singly-Linked-List-using-Template-(Data-Structure))




