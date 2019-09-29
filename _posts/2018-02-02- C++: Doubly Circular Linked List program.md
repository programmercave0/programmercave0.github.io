---
layout: post
title: "C++: Doubly Circular Linked List program"
tags:  [C++, Algorithm, Linked List, Data Structures]
date: 2018-02-02
---

In Doubly Linked List the node contains the address of the next node and previous node. 

And in Circular Linked List the last node contains the address of the first node(or head). 

Here, in Doubly Circular Linked List the last node contains the address of first node and first node contains the address of the last node. 

![Doubly Linked List]({{ site.url }}/assets/DLL.png)

In C++ implementation of this program we have also copy constructor, move constructor, copy assignment operator, move assignment operator and destructor.

Because the presence of a user-defined destructor, copy-constructor, or copy-assignment operator prevents implicit definition of the move constructor and the move assignment operator, any class for which move semantics are desirable, has to declare all five special member functions. This is called [Rule of Five](https://en.cppreference.com/w/cpp/language/rule_of_three).

```cpp
#include <iostream>
#include <utility>

template <typename T>
class circular_doubly_linked_list
{
  struct Node
  {
    T data;
    Node * next;
    Node * prev;
    Node(T value) : data(std::move(value)),
                    next(nullptr),
                    prev(nullptr)
                    {}
  };
  Node *head, *tail;

public:
  circular_doubly_linked_list() : head(nullptr),
                                  tail(nullptr)
                                  {}
  //copy constructor
  circular_doubly_linked_list(const circular_doubly_linked_list &);
  //copy assignment
  circular_doubly_linked_list& operator=(const circular_doubly_linked_list& cdll)
  {
    circular_doubly_linked_list  temp(cdll);
    temp.swap(*this);
    return *this;
  }
  //move constructor
  circular_doubly_linked_list(circular_doubly_linked_list&&) noexcept;
  //move assignment
  circular_doubly_linked_list& operator=(circular_doubly_linked_list&& cdll) noexcept
  {
    cdll.swap(*this);
    return *this;
  }
  ~circular_doubly_linked_list();

  void append_node(T);
  void delete_node(T);

  friend void swap(circular_doubly_linked_list& lhs, circular_doubly_linked_list& rhs)
  {
    std::swap(lhs.head, rhs.head);
  }

  template <typename U>
  friend std::ostream & operator<<(std::ostream & os, const circular_doubly_linked_list<U> & cdll)
  {
    cdll.print_list(os);
    return os;
  }

private:

  struct Node *search(T value)
  {
    Node *node = head;
    while(node->next != head)
    {
      if(node->data == value)
      {
        return node;
      }
      node = node->next;
    }
    if(node->data == value)
    {
      return node;
    }
    return nullptr;
  }

  void print_list(std::ostream& os = std::cout) const
  {
    Node *tmp = head;
    while(tmp->next != head)
    {
      std::cout << tmp->data << ' ';
      tmp = tmp->next;
    }
    std::cout << tmp->data << '\n';
  }
};

template <typename T>
circular_doubly_linked_list<T>::circular_doubly_linked_list(const circular_doubly_linked_list & cdll)
{
  if(cdll.head == nullptr)
  {
    head = tail = nullptr;
  }
  else
  {
    head = new Node(cdll.head->data);
    Node *curr = head;
    Node *tmp = head;
    Node *obj_curr = cdll.head;
    while(obj_curr->next != cdll.head)
    {
      curr->next = new Node(obj_curr->next->data);
      obj_curr = obj_curr->next;
      curr = curr->next;
      curr->prev = tmp;
      tmp = tmp->next;
    }
    tail = curr;
    curr->next = head;
    head->prev = curr;
  }
}

template <typename T>
circular_doubly_linked_list<T>::circular_doubly_linked_list(circular_doubly_linked_list&& cdll) noexcept
{
  head = tail = nullptr;
  swap(*this, cdll);
}

template <typename T>
void circular_doubly_linked_list<T>::append_node(T value)
{
  Node *node = new Node(std::move(value));

  if(head == nullptr)
  {
    node->next = node;
    node->prev = node;
    head = node;
    tail = node;
  }

  tail = head->prev;
  tail->next = node;
  node->prev = tail;
  node->next = head;
  head->prev = node;
  tail = node;
}

template <typename T>
void circular_doubly_linked_list<T>::delete_node(T value)
{
  Node *node = search(value);
  if(node == nullptr)
  {
    std::cerr << "No such value in the list\n";
    return;
  }
  else
  {
  Node *tmp = head;
  Node *tail = head->prev;
  if(tmp == node)
  {
    tail->next = tmp->next;
    tmp->prev->next->prev = tail;
    head = tail->prev;
    delete tmp;
    return;
  }
  else if(tail == node)
  {
    Node *curr = tail;
    tmp = tail->prev;
    tmp->next = curr->next;
    head->prev = tmp;
    tail = tmp;
    delete curr;
    return;
  }
  else
  {
    while(tmp->next != head)
    {
      if(tmp == node)
      {
        tmp->prev->next = tmp->next;
        tmp->prev->next->prev = tmp->prev;
        delete tmp;
        return;
      }
      tmp = tmp->next;
    }
  }
}

}

template <typename T>
circular_doubly_linked_list<T>::~circular_doubly_linked_list()
{
  if(head)
  {
    Node *tmp = head;
    while(tmp->next != head)
    {
      Node *t = tmp;
      tmp = tmp->next;
      delete t;
    }
    delete tmp;
    head = nullptr;
  }
}

int main()
{
  circular_doubly_linked_list<int> cdll1;
  cdll1.append_node(3);
  cdll1.append_node(4);
  cdll1.append_node(5);
  cdll1.append_node(6);
  cdll1.append_node(7);
  cdll1.append_node(8);
  std::cout << cdll1;
  cdll1.delete_node(6);
  std::cout << cdll1;
  circular_doubly_linked_list<int> cdll2(cdll1); // using copy constructor
  std::cout << "Linked List 2: " << cdll2;
  circular_doubly_linked_list<int> cdll3 = cdll1; //using copy assignment
  std::cout << "Linked List 3: " << cdll3;
  circular_doubly_linked_list<int> cdll4 = std::move(cdll2); //using move constructor
  std::cout << "Linked list 4: " << cdll4;
}
```
 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like

[C++: Move all Even numbers before Odd numbers in Singly Linked List (Using STL)](https://programmercave.github.io/blog/2018/02/08/C++-Move-all-Even-numbers-before-Odd-numbers-in-Singly-Linked-List-(Using-STL))

[C++: Merge two sorted Linked List (in-place)](https://programmercave.github.io/blog/2018/02/06/C++-Merge-two-sorted-Linked-List-(in-place))

[C++: Split Singly Circular Linked List program](https://programmercave.github.io/blog/2018/02/04/C++-Split-Singly-Circular-Linked-List-program)

[C++: Reverse the Linked List (Iterative Method) program](https://programmercave.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Iterative-Method)-program)

[C++:Reverse the Linked List (Recursive Method) program](https://programmercave.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Recursive-Method)-program)

[C++: Linked List containing Loop (Floyd Cycle finding Algorithm) program](https://programmercave.github.io/blog/2018/01/20/C++-Linked-List-containing-Loop-(Floyd-Cycle-finding-Algorithm)-program)

[C++: Swapping Node Links in Linked List](https://programmercave.github.io/blog/2017/08/23/C++-Swapping-Node-Links-in-Linked-List)

[C++: Queue implementation using Linked List (Data Structure)](https://programmercave.github.io/blog/2017/08/01/C++-Queue-implementation-using-Linked-List-(Data-Structure))

[C++: Stack implementation using LinkedList (Data Structure)](https://programmercave.github.io/blog/2017/07/31/C++-Stack-implementation-using-LinkedList-(Data-Structure))

[C++: Doubly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/28/C++-Doubly-Linked-List-using-Template-(Data-Structure))

[C++: Singly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/27/C++-Singly-Linked-List-using-Template-(Data-Structure))




