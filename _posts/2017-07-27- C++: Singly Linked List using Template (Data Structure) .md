---
layout: post
title: "C++: Singly Linked List using Template (Data Structure)"
tags:  [C++, Algorithm, Linked List]
date: 2017-07-27
---

A linked list is a data structure in which the objects are arranged in a linear order. Unlike an array, in which the linear order is determined by the array indices, the order in a linked list is determined by a pointer in each object. It has a group of nodes which together represents a sequence.  Under the simplest form, each node is composed of data and a reference (in other words, a link) to the next node in the sequence. 

Adavantages:

➧ Linked lists are a dynamic data structure, which can grow and be pruned, allocating and deallocating memory while the program is running. 
➧ Insertion and deletion node operations are easily implemented in a linked list.
➧ Dynamic data structures such as stacks and queues can be implemented using a linked list.
➧ There is no need to define an initial size for a linked list. 
➧ Items can be added or removed from the middle of list. 
➧ Backtracking is possible in two way linked list.

![Single Linked List]({{ site.url }}/assets/ll1.png)

C++ Implementation

```cpp
#include <iostream>

template <class T>
class LinkedList
{
  struct Node
  {
    T data;
    Node * next;
    Node(T val): data(val), next(nullptr){}
  };
  Node *head;

 public:
     LinkedList() : head(nullptr){}
     LinkedList(const LinkedList<T> & ll) = delete;
     LinkedList& operator=(LinkedList const&) = delete;
    ~LinkedList();
     void insert(T);
     void display(std::ostream& out = std::cout) const
     {
Node *node=head;
while(node!=nullptr)
    {
        out << node->data << " ";
        node=node->next;
    }
}
     void deleteNode(T);
     template <class U>
     friend std::ostream & operator<<(std::ostream & os, const LinkedList<U> & ll);

 private:
    struct Node *search(T n)
    {                            //returns node of the given number
        Node *node = head;
        while(node!=nullptr)
            {
            if(node->data==n)
                 return node;
        node=node->next;
            }
        std::cerr<<"No such element in the list \n";
        return nullptr;
    }

};

template <class U>
std::ostream & operator<<(std::ostream & os, const LinkedList<U>& ll)
{
 ll.display(os);
 return os;
}

template <class T>
void LinkedList<T>::insert(T data)
{
  Node *t = new Node(data);
  Node *tmp = head;
  if (tmp == nullptr)
    {
      head = t;
    }
  else
    {
      while (tmp->next != nullptr)
      {
          tmp = tmp->next;
      }
      tmp->next = t;
    }
}

template <class T>
void LinkedList<T>::deleteNode(T data)
{
  Node *node=search(data);
  Node *tmp = head;
  if(tmp==node)
   {
    head=tmp->next;
   }
   else
   {
   while(node!=nullptr)
   {
      if(tmp->next==node)
      {
        tmp->next=node->next;
        return ;
      }
      tmp=tmp->next;
    }
  }
      delete tmp;
}

template <class T>
LinkedList<T>::~LinkedList()
{
  Node *tmp = nullptr;
  while (head)
  {
      tmp = head;
      head = head->next;
      delete tmp;
   }
  head =nullptr;
}

int main()
{
    LinkedList<int> ll1;
    ll1.insert(5);
    ll1.insert(6);
    ll1.insert(7);
    ll1.insert(8);
    std::cout<<ll1<<std::endl;
    ll1.deleteNode(7);
    std::cout<<ll1<<std::endl;
    LinkedList<char> ll2;
    ll2.insert('a');
    ll2.insert('r');
    ll2.insert('d');
    ll2.insert('y');
    std::cout<<ll2<<std::endl;
    return 0;
}
```

We have  used  `LinkedList(const LinkedList<T> & ll) = delete;` and `LinkedList& operator=(LinkedList const&) = delete;`
Because they don't allow compiler to default generate it and any invocation of those functions make the program ill-formed (aka compiler will print an error saying the function has been deleted, thus not callable).

We have also written `void display(std::ostream& out = std::cout) const` and `friend std::ostream & operator<<(std::ostream & os, const LinkedList<U> & ll);` means the friend declaration here firstly declares a non-member function, and makes it friend of the class, meaning it can access the private and protected members of the class LinkedList. 
 
Note: The standard way of printing things in C++ is to use `operator<<` so we can print a list using it. 

