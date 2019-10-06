---
layout: post
title: "C++: Linked List containing Loop (Floyd Cycle finding Algorithm) program"
tags:  [C++, Algorithm, Linked List, Data Structures]
date: 2018-01-20
---

Given a Linked List, we have to find does loop exist in Linked List and if yes, find the length of loop.

![Floyd Cycle Finding Algorithm]({{ site.url }}/assets/FlyodCycle1.png)

We are using [Floyd cycle finding algorithm](https://en.wikipedia.org/wiki/Cycle_detection). In this we have two pointers `slowPtr` and `fastPtr` which moves with different speed. Once they enter the loop, they are expected to meet, which denotes that there is a loop.

![Floyd Cycle Finding Algorithm]({{ site.url }}/assets/FlyodCycle2.png)

C++ Implementation

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
  LinkedList& operator=(const LinkedList& ll) = delete; //copy assignment
  ~LinkedList();
  void insert(T);
  void createLoop(int);
  int lengthOfLoop();
  Node* doesLoopExist()
  {
    Node *slowPtr = head;
    Node *fastPtr = head;

    while(slowPtr && fastPtr && fastPtr->next)
    {
      slowPtr = slowPtr->next;
      fastPtr = fastPtr->next->next;

      if(slowPtr == fastPtr)
      {
        return slowPtr;
      }
    }
    return nullptr;
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
void LinkedList<T>::createLoop(int n)
{
  Node *tmp = head;
  Node *tail = head;
  for(int i = 0; i < n-1; i++)
  {
    tmp = tmp->next;
  }

  while(tail->next != nullptr)
  {
    tail = tail->next;
  }
  tail->next = tmp;
}

template <class T>
int LinkedList<T>::lengthOfLoop()
{
  int count = 0;
  bool loopExist = false;
  Node *slowPtr = nullptr, *fastPtr = nullptr;

  slowPtr = doesLoopExist();
  fastPtr = slowPtr;
  if(slowPtr != nullptr)
  {
    loopExist = true;
  }
  else
  {
    return 0;
  }
  if(loopExist)
  {
    fastPtr = fastPtr->next;
    count++;
    while(slowPtr != fastPtr)
    {
      fastPtr = fastPtr->next;
      count++;
    }
    return count;
  }
  return 0;
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
  ll1.insert('m');
  ll1.insert('e');
  ll1.insert('r');

  int nodeNumber = 5;
  //Node number starts from 1

  ll1.createLoop(nodeNumber);
  bool result = ll1.doesLoopExist();
  if(result == true)
  {
    std::cout <<"Loop exist in the Linked List\n";
  }
  else
  {
    std::cout <<"Loop does not exist in the Linked List\n";
  }

  int len = ll1.lengthOfLoop();
  std::cout << "Length of Loop is " << len <<"\n";

  exit(0);
}
```
`std::move` is used to indicate that an object t may be "moved from", i.e. allowing the efficient transfer of resources from t to another object. 
It is defined in header `<utility>`.

You may also like:
[C++: Move all Even numbers before Odd numbers in Singly Linked List (Using STL)](https://programmercave0.github.io/blog/2018/02/08/C++-Move-all-Even-numbers-before-Odd-numbers-in-Singly-Linked-List-(Using-STL))

[C++: Merge two sorted Linked List (in-place)](https://programmercave0.github.io/blog/2018/02/06/C++-Merge-two-sorted-Linked-List-(in-place))

[C++: Split Singly Circular Linked List program](https://programmercave0.github.io/blog/2018/02/04/C++-Split-Singly-Circular-Linked-List-program)

[C++: Doubly Circular Linked List program](https://programmercave0.github.io/blog/2018/02/02/C++-Doubly-Circular-Linked-List-program)

[C++: Reverse the Linked List (Iterative Method) program](https://programmercave0.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Iterative-Method)-program)

[C++:Reverse the Linked List (Recursive Method) program](https://programmercave0.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Recursive-Method)-program)

[C++: Swapping Node Links in Linked List](https://programmercave0.github.io/blog/2017/08/23/C++-Swapping-Node-Links-in-Linked-List)

[C++: Queue implementation using Linked List (Data Structure)](https://programmercave0.github.io/blog/2017/08/01/C++-Queue-implementation-using-Linked-List-(Data-Structure))

[C++: Stack implementation using LinkedList (Data Structure)](https://programmercave0.github.io/blog/2017/07/31/C++-Stack-implementation-using-LinkedList-(Data-Structure))

[C++: Doubly Linked List using Template (Data Structure)](https://programmercave0.github.io/blog/2017/07/28/C++-Doubly-Linked-List-using-Template-(Data-Structure))

[C++: Singly Linked List using Template (Data Structure)](https://programmercave0.github.io/blog/2017/07/27/C++-Singly-Linked-List-using-Template-(Data-Structure))









