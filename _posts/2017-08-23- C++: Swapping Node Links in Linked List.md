---
layout: post
title: "C++: Swapping Node Links in Linked List"
tags:  [C++, Algorithm, Linked List, Data Structure]
date: 2017-08-23
---

Here we are going to swap the links of the node instead of data in the node. 

Here we are using `swapLinks()` function to swap the links of the node. We are using `search()` function which returns the node of the value entered and `insert()` to insert the values.  

For eg:
```
LinkedList1 = {10, 15, 12, 13, 28, 14, 16}
swapLinks(10, 13)
LinkedList1 = {13, 15, 12, 10, 28, 14, 16}
```

C++ Implementation

```cpp
#include <iostream>
#include <string>

template<class T>
class LinkedList
{
struct Node{
  T data;
  Node * next;
  Node(T val): data(val), next(nullptr){}
};

Node* head;

public:

  LinkedList() : head(nullptr) {}

  void insert(T val)
  {
    Node* node = new Node(val);
    Node* tmp  = head;

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

  void swapLinks(T val1, T val2)
  {
    Node* prevNode1 = nullptr;
    Node* prevNode2 = nullptr;
    Node* nextNode1 = nullptr;
    Node* nextNode2 = nullptr;
    Node* node      = head;
    Node* node1     = search(val1);
    Node* node2     = search(val2);
    Node* tmp;
    //if val1 is head node
    if(node1 == head)
    {
      nextNode1 = node1->next;

      while(node->next != node2)
      {
        node = node->next;
      }
      if(node->next == node2)
      {
        prevNode2 = node;
        nextNode2 = node2->next;
      }

      tmp             = node1;
      head            = node2;
      node2->next     = nextNode1;
      prevNode2->next = node1;
      node1->next     = nextNode2;
    }
    //if val2 is head node
    else if(node2 == head)
    {
      nextNode2 = node2->next;

      while(node->next != node1)
      {
        node = node->next;
      }
      if(node->next == node1)
      {
        prevNode1 = node;
        nextNode1 = node1->next;
      }

      tmp             = node2;
      head            = node1;
      node1->next     = nextNode2;
      prevNode1->next = node2;
      node2->next     = nextNode1;
    }
    //neirher val1 is head node nor val2
    else
    {
      while(node->next != node1)
      {
        node = node->next;
      }
      if(node->next == node1)
      {
        prevNode1 = node;
        nextNode1 = node1->next;
      }

      node = head;

      while(node->next != node2)
      {
        node = node->next;
      }
      if(node->next == node2)
      {
        prevNode2 = node;
        nextNode2 = node2->next;
      }

      prevNode1->next = node2;
      tmp             = node2->next;
      node2->next     = node1->next;
      prevNode2->next = node1;
      node1->next     = tmp;
    }
  }

  friend std::ostream & operator <<(std::ostream & os, LinkedList<T>& ll)
  {
    ll.display(os);
    return os;
  }

private:

  struct Node *search(T val)
  {
    Node* node = head;
    while(node != nullptr)
    {
      if(node->data == val)
      {
        return node;
      }
      node = node->next;
    }
    std::cerr << "No such element in the List \n";
    return nullptr;
  }

  void display(std::ostream& out = std::cout) const
  {
    Node* node = head;
    while(node != nullptr)
    {
      out << node->data <<" ";
      node = node->next;
    }
  }
};

int main()
{
  LinkedList<int> ll1;
  ll1.insert(10);
  ll1.insert(15);
  ll1.insert(12);
  ll1.insert(13);
  ll1.insert(28);
  ll1.insert(14);
  ll1.insert(16);
  std::cout << "LinkedList1 : "<< ll1 <<"\n";
  ll1.swapLinks(12, 16);
  std::cout << ll1 <<"\n";
  LinkedList<char> ll2;
  ll2.insert('t');
  ll2.insert('r');
  ll2.insert('f');
  ll2.insert('x');
  ll2.insert('a');
  ll2.insert('y');
  ll2.insert('n');
  std::cout << "LinkedList2 : "<< ll2 <<"\n";
  ll2.swapLinks('t', 'a');
  std::cout << ll2 <<"\n";
  LinkedList<std::string> ll3;
  ll3.insert("apple");
  ll3.insert("dog");
  ll3.insert("car");
  ll3.insert("code");
  std::cout << "LinkedList3 : "<< ll3 <<"\n";
  ll3.swapLinks("apple", "car");
  std::cout << ll3 <<"\n";
  return 0;
}
```

You may also like:
[C++: Move all Even numbers before Odd numbers in Singly Linked List (Using STL)](https://programmercave.github.io/blog/2018/02/08/C++-Move-all-Even-numbers-before-Odd-numbers-in-Singly-Linked-List-(Using-STL))

[C++: Merge two sorted Linked List (in-place)](https://programmercave.github.io/blog/2018/02/06/C++-Merge-two-sorted-Linked-List-(in-place))

[C++: Split Singly Circular Linked List program](https://programmercave.github.io/blog/2018/02/04/C++-Split-Singly-Circular-Linked-List-program)

[C++: Doubly Circular Linked List program](https://programmercave.github.io/blog/2018/02/02/C++-Doubly-Circular-Linked-List-program)

[C++: Reverse the Linked List (Iterative Method) program](https://programmercave.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Iterative-Method)-program)

[C++:Reverse the Linked List (Recursive Method) program](https://programmercave.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Recursive-Method)-program)

[C++: Linked List containing Loop (Floyd Cycle finding Algorithm) program](https://programmercave.github.io/blog/2018/01/20/C++-Linked-List-containing-Loop-(Floyd-Cycle-finding-Algorithm)-program)

[C++: Queue implementation using Linked List (Data Structure)](https://programmercave.github.io/blog/2017/08/01/C++-Queue-implementation-using-Linked-List-(Data-Structure))

[C++: Stack implementation using LinkedList (Data Structure)](https://programmercave.github.io/blog/2017/07/31/C++-Stack-implementation-using-LinkedList-(Data-Structure))

[C++: Doubly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/28/C++-Doubly-Linked-List-using-Template-(Data-Structure))

[C++: Singly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/27/C++-Singly-Linked-List-using-Template-(Data-Structure))



