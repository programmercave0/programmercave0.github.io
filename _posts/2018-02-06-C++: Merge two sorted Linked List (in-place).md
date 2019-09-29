---
layout: post
title: "C++: Merge two sorted Linked List (in-place)"
tags:  [C++, Algorithm, Linked List, Data Structures]
date: 2018-02-06
---

Given two sorted Linked List, we have to merge them without using another linked list.

First we will create a *node* and this *node* will be the *node* which contains the smallest element in the two lists.

Since the lists are sorted in ascending order, so *first nodes* will contain smallest element.

Then it will point to the next greatest element in the two given lists. 

After, lists merge, this *node* will become *head* of the merged list.

C++ Implementation

```cpp
#include <iostream>
#include <utility>
#include <cassert>

template<class T>
class LinkedList
{
  struct Node
  {
    T data;
    Node * next = nullptr;
    Node()          : data(), next(nullptr) {}
    Node(T value)   : data(std::move(value)), next(nullptr) {}
  };

  Node *head;

public:
  LinkedList() : head(nullptr) {}
  LinkedList(const LinkedList& ll) = delete; //copy constructor
  LinkedList& operator=(const LinkedList& ll) = delete; //copy assignment
  ~LinkedList()
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

  void insert(T);
  void mergeSortedList(LinkedList<T>&);
  void printList() const;

private:
  static void advance(Node*& node)
  {
    assert(node != nullptr);
    node = node->next;
  }

  Node* getLastNode(Node*& node)
  {
    while(node->next != nullptr)
      node = node->next;

    return node;
  }
};

template <class T>
void LinkedList<T>::insert(T value)
{
 Node *node = new Node(std::move(value));
 Node *tmp = head;
 if(tmp == nullptr)
 {
   head = node;
 }
 else
 {
   tmp = getLastNode(tmp);
   tmp->next = node;
 }
}

template <class T>
void LinkedList<T>::mergeSortedList(LinkedList<T>& ll2)
{
  if(ll2.head == nullptr)
    return;

  if(head == nullptr)
  {
    head = ll2.head;
    ll2.head = nullptr;
    return;
  }

  Node *head1 = head;
  Node *head2 = ll2.head;

  Node *node;

  if(head1->data <= head2->data)
  {
    node = head1;
    advance(head1);
  }
  else if(head2->data <= head1->data)
  {
    node = head2;
    advance(head2);
  }

  Node *tmp = nullptr;
  tmp = node;

  while(head1 != nullptr && head2 != nullptr)
  {
    if(head1->data <= head2->data)
    {
      tmp->next = head1;
      advance(head1);
    }
    else
    {
      tmp->next = head2;
      advance(head2);
    }

    advance(tmp);
  }

  //if there are extra nodes in either list
  if(head1 != nullptr)
    tmp->next = head1;

  if(head2 != nullptr)
     tmp->next = head2;

  head = node;
  ll2.head = nullptr;
}

template <class T>
void LinkedList<T>::printList() const
{
  if(head == nullptr)
  {
    std::cout << "Empty List \n";
    return;
  }

  Node *node = head;

  while(node != nullptr)
  {
    std::cout << node->data << " ";
    advance(node);
  }

  std::cout << "\n";
}

int main()
{
  LinkedList<int> ll1;
  ll1.insert(5);
  ll1.insert(10);
  ll1.insert(18);
  ll1.insert(25);
  std::cout << "List 1 : ";
  ll1.printList();

  LinkedList<int> ll2;
  ll2.insert(6);
  ll2.insert(8);
  ll2.insert(11);
  ll2.insert(20);
  ll2.insert(23);
  ll2.insert(28);
  ll2.insert(40);
  std::cout << "List 2 : ";
  ll2.printList();

  ll1.mergeSortedList(ll2);
  std::cout << "Merged List : ";
  ll1.printList();
}
```

Output

![Output]({{ site.url }}/assets/MergeSortedLLOut.png)

 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like

[C++: Move all Even numbers before Odd numbers in Singly Linked List (Using STL)](https://programmercave.github.io/blog/2018/02/08/C++-Move-all-Even-numbers-before-Odd-numbers-in-Singly-Linked-List-(Using-STL))

[C++: Split Singly Circular Linked List program](https://programmercave.github.io/blog/2018/02/04/C++-Split-Singly-Circular-Linked-List-program)

[C++: Doubly Circular Linked List program](https://programmercave.github.io/blog/2018/02/02/C++-Doubly-Circular-Linked-List-program)

[C++: Reverse the Linked List (Iterative Method) program](https://programmercave.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Iterative-Method)-program)

[C++:Reverse the Linked List (Recursive Method) program](https://programmercave.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Recursive-Method)-program)

[C++: Linked List containing Loop (Floyd Cycle finding Algorithm) program](https://programmercave.github.io/blog/2018/01/20/C++-Linked-List-containing-Loop-(Floyd-Cycle-finding-Algorithm)-program)

[C++: Swapping Node Links in Linked List](https://programmercave.github.io/blog/2017/08/23/C++-Swapping-Node-Links-in-Linked-List)

[C++: Queue implementation using Linked List (Data Structure)](https://programmercave.github.io/blog/2017/08/01/C++-Queue-implementation-using-Linked-List-(Data-Structure))

[C++: Stack implementation using LinkedList (Data Structure)](https://programmercave.github.io/blog/2017/07/31/C++-Stack-implementation-using-LinkedList-(Data-Structure))

[C++: Doubly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/28/C++-Doubly-Linked-List-using-Template-(Data-Structure))

[C++: Singly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/27/C++-Singly-Linked-List-using-Template-(Data-Structure))




