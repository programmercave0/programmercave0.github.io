---
layout: post
title: "C++: Move all Even numbers before Odd numbers in Singly Linked List (Using STL)"
tags:  [C++, Algorithm, Linked List, Standard Tempelate Library, Data Structures]
date: 2018-02-08
---

Given a Singly Linked List, we have to modify it such that all Even numbers appear before Odd numbers. 


For eg. 
```
Given Linked List : 1, 2, 3, 4, 5, 6, 7
After Modification : 2, 4, 6, 1, 3, 5, 7
```

The functions used are ```insert()```, ```exchangeEvenOdd()```, ```getLastNode()```, ```isOdd()```, and ```printList()```.

```cpp
#include <iostream>
#include <utility>
#include <cassert>

class LinkedList
{
  struct Node
  {
    int data;
    Node * next = nullptr;
    Node(int value)   : data(std::move(value)), next(nullptr) {}
  };
  Node *head;

public:
  LinkedList() : head(nullptr) {}
  ~LinkedList()
  {
    Node *tmp = nullptr;
    while (head)
    {
      tmp = head;
      head = head->next;
      delete tmp;
    }
    head = nullptr;
  }

  void insert(int);
  void exchangeEvenOdd();
  void printList() const;

private:
  static void advance(Node*& node)
  {
    assert (node != nullptr);
    node = node->next;
  }

  Node* getLastNode()
  {
    Node *node = head;
    while (node->next != nullptr)
           node = node->next;

    return node;
  }

  bool isOdd(int num)
  {
    if (num % 2 != 0)
        return true;
    else
        return false;
  }
};

void LinkedList::insert(int value)
{
 Node *node = new Node(std::move(value));
 Node *tmp = head;
 if (tmp == nullptr)
 {
   head = node;
 }
 else
 {
   tmp = getLastNode();
   tmp->next = node;
 }
}

void LinkedList::exchangeEvenOdd()
{
  Node *node = nullptr;
  Node *lastNodeToTest = getLastNode();
  Node *tail = lastNodeToTest;

  while (isOdd(head->data) == true)
  {
    node = head;
    advance(head);
    tail->next = node;
    advance(tail);
  }

  Node *tmp = head;
  Node *curr = head;

  while (tmp->next != lastNodeToTest)
  {
    if (isOdd(curr->next->data) == true)
    {
      node = curr->next;
      curr->next = node->next;
      tail->next = node;
      advance(tail);
    }
    else
    {
      //advance "curr" and "tmp" only when next node to it is even
      advance(curr);
      advance(tmp);
    }
  }

  if (isOdd(curr->next->data) == true && tmp->next == lastNodeToTest)
  {
    node = lastNodeToTest;
    curr->next = lastNodeToTest->next;
    tail->next = lastNodeToTest;
    advance(tail);
  }
  tail->next = nullptr;
  lastNodeToTest = nullptr;
  node = nullptr;
}

void LinkedList::printList() const
{
  if (head == nullptr)
  {
    std::cout << "Empty List \n";
    return;
  }

  Node *node = head;

  while (node != nullptr)
  {
    std::cout << node->data << " ";
    advance(node);
  }

  std::cout << "\n";
}

int main()
{
  LinkedList ll1;
  ll1.insert(1);
  ll1.insert(2);
  ll1.insert(3);
  ll1.insert(4);
  ll1.insert(5);
  ll1.insert(6);
  ll1.insert(7);
  std::cout << "Original List : ";
  ll1.printList();

  ll1.exchangeEvenOdd();
  std::cout << "New List : ";
  ll1.printList();
}
```

We can also use [std::stable_partiton](https://en.cppreference.com/w/cpp/algorithm/stable_partition) and [std::partition](https://en.cppreference.com/w/cpp/algorithm/partition) but in std::partition output will not be in the same relative order of each group.

```cpp
#include <iostream>
#include <algorithm>
#include <list>
#include <iterator>

int main()
{
    std::list<int> list {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

    std::cout << "Original list : ";
    std::copy(std::begin(list), std::end(list), std::ostream_iterator<int>(std::cout, " "));
    std::cout << "\n";

    std::stable_partition(std::begin(list), std::end(list), [](int val){return val % 2 == 0;});
    std::cout << "New list      : ";
    std::copy(std::begin(list), std::end(list), std::ostream_iterator<int>(std::cout, " "));
    std::cout << "\n";
}
```

 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like

[C++: Merge two sorted Linked List (in-place)](https://programmercave.github.io/blog/2018/02/06/C++-Merge-two-sorted-Linked-List-(in-place))

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








