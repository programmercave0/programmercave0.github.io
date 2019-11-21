---
layout: post
title: "Singly Linked List | C++ Implementation"
tags:  [C++, Algorithm, Linked List, Data Structures]
date: 2017-07-27
---

The nodes in a linked list are connected through *pointers*. Pointers represent the address of a location in a memory. The order in a linked list is determined by a pointer in each node. A *node* in a **singly linked** list contains a data item and a node pointer to the next node. In a singly linked list we can traverse only in one direction.

The first node of the linked list is the head and the last node is the tail. If head is NULL then the list is empty.

In C++, a node can be defined using `struct`, which contain a data item and a pointer to next node.

```cpp
struct Node
{
    T data;
    Node * next;
    Node(T val): data(val), next(nullptr){}
};
 ```   

`Node(T val): data(val), next(nullptr){}` is the constructor for the `struct Node` which is used to initialise `data` and `next`.  `T` means it is a generic struct and data can store values of all data types.

To declare head: `Node *head;`

![Singly Linked List]({{ site.url }}/assets/singlylinkedlist.png){:class="img-responsive"}

In the above fig. Node containing 5 is head, node containing 15 is tail and its next pointer points to nullptr.

<h1>Implementation</h1>
The three basic operations supported by a linked list are searching, insertion and deletion.

<h3>Searching</h3>

In the `search` function a value is passed as an argument and its node is returned if found, otherwise a message says “No such element in the list” and `nullptr` is returned.

The function starts searching from the head to the last node and passed value is matched with every node’s data item. 

Here is the code for iterative search.

```cpp
struct Node *search(T n)
{                            //returns node of the given value
    Node *node = head;
    while(node != nullptr)
    {
        if(node->data == n)
            return node;
        node = node->next;
    }

    std::cerr << "No such element in the list \n";
    return nullptr;
}
```    
   
<h3>Insertion</h3>

`insert` function insert a node with the value at the end of the linked list. If the linked list does not contain any node then the new node becomes head.

```cpp
void insert(T data)
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
```

We can also insert node at the front and make that new node as head.

<h3>Deletion</h3>

In `deleteNode` function the value is entered which is to be deleted. The function search the node containing the value using `search` function and then the node is deleted. 

{% include ads.html %}<br/>

If the searched node is `head` then node next to head is made head and then the searched node is deleted. The node is deleted only if the value exists means `if (node != nullptr)`.

```cpp
void deleteNode(T data)
{
    Node *node=search(data);
    Node *tmp = head;

    if(tmp == node)
    {
        head=tmp->next;
    }
    else if (node != nullptr)
    {
        while(node != nullptr)
        {
            if(tmp->next==node)
            {
                tmp->next=node->next;
                return ;
            }
            tmp=tmp->next;
        }
        delete tmp;
    }
}
```

<h3>C++ Implementation of Singly Linked List</h3>

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
          Node *node = head;
          while(node != nullptr)
          {
              out << node->data << " ";
              node = node->next;
          }
      }
      
     void deleteNode(T);
     template <class U>
     friend std::ostream & operator<<(std::ostream & os, const LinkedList<U> & ll);

 private:
    struct Node *search(T n)
    {                            //returns node of the given value
        Node *node = head;
        while(node != nullptr)
        {
            if(node->data == n)
                 return node;
            node = node->next;
        }

        std::cerr << "No such element in the list \n";
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

    if(tmp == node)
    {
        head=tmp->next;
    }
    else if (node != nullptr)
    {
        while(node != nullptr)
        {
            if(tmp->next==node)
            {
                tmp->next=node->next;
                return ;
            }
            tmp=tmp->next;
        }
        delete tmp;
    }
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
    ll1.deleteNode(11);
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

View this code on [Github](https://github.com/programmercave0/Algo-Data-Structure/blob/master/Singly%20Linked%20List/C++/linkedlist.cpp)

We have  used  `LinkedList(const LinkedList<T> & ll) = delete;` and `LinkedList& operator=(LinkedList const&) = delete;`
Because they don't allow compiler to default generate it and any invocation of those functions make the program ill-formed (aka compiler will print an error saying the function has been deleted, thus not callable).

We have also written `void display(std::ostream& out = std::cout) const` and `friend std::ostream & operator<<(std::ostream & os, const LinkedList<U> & ll);` means the friend declaration here firstly declares a non-member function, and makes it friend of the class, meaning it can access the private and protected members of the class LinkedList. 
 
Note: The standard way of printing things in C++ is to use `operator<<` so we can print a list using it. 

Get this post in pdf - [Singly Linked List](https://www.file-up.org/2k1y52ykbajk)

Reference:<br/>
[Introduction to Algorithms](https://amzn.to/2OarGBs)<br/>
[The Algorithm Design Manual](https://amzn.to/2CH9h9Z)<br/>
[Data Structures and Algorithms Made Easy](https://amzn.to/2NLM0dd)<br/>

<h3>You may also like</h3>
[Move all Odd numbers after Even numbers in Singly Linked List](https://programmercave0.github.io/blog/2018/02/08/C++-Move-all-Even-numbers-before-Odd-numbers-in-Singly-Linked-List-(Using-STL))<br/>
[Merge two sorted Linked List (in-place)](https://programmercave0.github.io/blog/2018/02/06/C++-Merge-two-sorted-Linked-List-(in-place))<br/>
[Split Singly Circular Linked List program](https://programmercave0.github.io/blog/2018/02/04/C++-Split-Singly-Circular-Linked-List-program)<br/>
[Doubly Circular Linked List program](https://programmercave0.github.io/blog/2018/02/02/C++-Doubly-Circular-Linked-List-program)<br/>
[Reverse the Linked List](https://programmercave0.github.io/blog/2018/01/23/C++-Reverse-the-Linked-List-(Iterative-Method)-program)<br/>
[Finding Length of Loop in Linked List](https://programmercave0.github.io/blog/2018/01/20/C++-Linked-List-containing-Loop-(Floyd-Cycle-finding-Algorithm)-program)<br/>
[Doubly Linked List using Template](https://programmercave0.github.io/blog/2017/07/28/C++-Doubly-Linked-List-using-Template-(Data-Structure))




