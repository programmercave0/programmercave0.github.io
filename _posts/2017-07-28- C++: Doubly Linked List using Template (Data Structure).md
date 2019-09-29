---
layout: post
title: "C++: Doubly Linked List using Template (Data Structure) "
tags:  [C++, Algorithm, Linked List, Data Structures]
date: 2017-07-28
---

A doubly linked list is a linked data structure that consists of a set of sequentially linked records called nodes. It consists of an attribute *key* and two other pointer attributes: *next* and *prev*.The two node links allow traversal of the list in either direction. While adding or removing a node in a doubly linked list requires changing more links than the same operations on a singly linked list, the operations are simpler and potentially more efficient (for nodes other than first nodes) because there is no need to keep track of the previous node during traversal or no need to traverse the list to find the previous node, so that its link can be modified.

![Double Linked List]({{ site.url }}/assets/dll1.png)

C++ Implementation

```cpp
#include <iostream>

template<class T>
class DoublyLinkedList{
  struct Node{
  T data;
  Node* next;
  Node* prev;
  Node(T val):data(val),next(nullptr),prev(nullptr){}
  };
  Node *head ,*tail;

  public:
      DoublyLinkedList():head(nullptr),tail(nullptr){}

     ~DoublyLinkedList(){
        Node *tmp = nullptr;
          while (head){
             tmp = head;
             head = head->next;
             delete tmp;
          }
          head =nullptr;
        }

      DoublyLinkedList(const DoublyLinkedList<T> & dll) = delete;
      DoublyLinkedList& operator=(DoublyLinkedList const&) = delete;
      void insertFront(T val){
         Node *node = new Node(val);
          Node *tmp = head;
          if (head == nullptr){
              head = node;
              tail=node;
              }
         else{
            node->next=head;
            head=node;
            node->next->prev = node;
             while (tmp->next != nullptr){
               tmp = tmp->next;
             }
          tail=tmp;
        }
    }

      void insertBack(T val){
         Node *node=new Node(val);
           if(tail->next==nullptr){
            tail->next=node;
            tail=node;
            }
     }


     void deleteVal(T val){
      Node* find = findVal(val);
      Node *tmp = head;
        if(tmp==find){
          head=tmp->next;
        }
        else{
           while(find != nullptr){
              if(tmp->next==find){
              tmp->next = find->next;
              find->next->prev = tmp;
              delete find;
              return;
              }
            tmp=tmp->next;
          }
       }

    }

     template <class U>
     friend std::ostream & operator<<(std::ostream & os, const DoublyLinkedList<U> & dll){
      dll.display(os);
      return os;
     }

     private:

         Node *findVal(T n){    //returns node of the given number
         Node *node = head;
         while(node!=nullptr){
            if(node->data==n)
                 return node;
         node=node->next;
         }
         std::cerr<<"No such element in the list \n";
         return nullptr;
       }

       void display(std::ostream& out = std::cout) const{
        Node *node = head;
        while(node!=nullptr){
        out <<node->data << " ";
        node=node->next;
        }
      }
};
int main(){
  DoublyLinkedList<int> l1;
  l1.insertFront(3);
  l1.insertBack(5);
  l1.insertBack(12);
  l1.insertFront(6);
  l1.insertBack(88);
  std::cout<<l1<<"\n";
  l1.deleteVal(12);
   std::cout<<l1<<"\n";
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

[C++: Swapping Node Links in Linked List](https://programmercave.github.io/blog/2017/08/23/C++-Swapping-Node-Links-in-Linked-List)

[C++: Queue implementation using Linked List (Data Structure)](https://programmercave.github.io/blog/2017/08/01/C++-Queue-implementation-using-Linked-List-(Data-Structure))

[C++: Stack implementation using LinkedList (Data Structure)](https://programmercave.github.io/blog/2017/07/31/C++-Stack-implementation-using-LinkedList-(Data-Structure))

[C++: Singly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/27/C++-Singly-Linked-List-using-Template-(Data-Structure))




