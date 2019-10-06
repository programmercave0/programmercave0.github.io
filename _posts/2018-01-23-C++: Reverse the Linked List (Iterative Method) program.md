---
layout: post
title: "C++: Reverse the Linked List (Iterative Method) program"
tags:  [C++, Algorithm, Linked List, Data Structures]
date: 2018-01-23
---

Time Complexity : O(n)
Space Complexity : O(1) 

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
  void iterativeReverse()
  {
    head = iterativeReverse(head);
  }
private:
  Node* iterativeReverse(Node* head)
  {
    Node *previous = nullptr;
    Node *nextNode = nullptr;
    while(head)
    {
      nextNode = head->next;
      head->next = previous;
      previous = head;
      head = nextNode;
    }
    return previous;
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
  LinkedList<char> ll1;
  ll1.insert('p');
  ll1.insert('r');
  ll1.insert('o');
  ll1.insert('g');
  ll1.insert('r');
  ll1.insert('a');
  ll1.insert('m');
  ll1.printList();
  ll1.iterativeReverse();
  ll1.printList();
}
```

You may also like:
[C++: Move all Even numbers before Odd numbers in Singly Linked List (Using STL)](https://programmercave0.github.io/blog/2018/02/08/C++-Move-all-Even-numbers-before-Odd-numbers-in-Singly-Linked-List-(Using-STL))<br/>
[C++: Merge two sorted Linked List (in-place)](https://programmercave0.github.io/blog/2018/02/06/C++-Merge-two-sorted-Linked-List-(in-place))<br/>
[C++: Split Singly Circular Linked List program](https://programmercave0.github.io/blog/2018/02/04/C++-Split-Singly-Circular-Linked-List-program)<br/>
[C++: Doubly Circular Linked List program](https://programmercave0.github.io/blog/2018/02/02/C++-Doubly-Circular-Linked-List-program)<br/>
[C++:Reverse the Linked List (Recursive Method) program](https://programmercave0.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Recursive-Method)-program)<br/>
[C++: Linked List containing Loop (Floyd Cycle finding Algorithm) program](https://programmercave0.github.io/blog/2018/01/20/C++-Linked-List-containing-Loop-(Floyd-Cycle-finding-Algorithm)-program)<br/>
[C++: Swapping Node Links in Linked List](https://programmercave0.github.io/blog/2017/08/23/C++-Swapping-Node-Links-in-Linked-List)<br/>
[C++: Queue implementation using Linked List (Data Structure)](https://programmercave0.github.io/blog/2017/08/01/C++-Queue-implementation-using-Linked-List-(Data-Structure))<br/>
[C++: Stack implementation using LinkedList (Data Structure)](https://programmercave0.github.io/blog/2017/07/31/C++-Stack-implementation-using-LinkedList-(Data-Structure))<br/>
[C++: Doubly Linked List using Template (Data Structure)](https://programmercave0.github.io/blog/2017/07/28/C++-Doubly-Linked-List-using-Template-(Data-Structure))<br/>
[C++: Singly Linked List using Template (Data Structure)](https://programmercave0.github.io/blog/2017/07/27/C++-Singly-Linked-List-using-Template-(Data-Structure))<br/>




