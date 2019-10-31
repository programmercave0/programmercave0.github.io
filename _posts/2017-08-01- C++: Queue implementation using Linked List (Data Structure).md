---
layout: post
title: "C++: Queue implementation using Linked List (Data Structure)"
tags:  [C++, Algorithm, Linked List, Queue, Data Structures]
date: 2017-08-01
---

Queue:

➧ It is a Dynamic set. Dynamic sets are sets which can be manipulated by algorithms, which can grow, shrink or change over time.

➧ It implements first-in,first-out means element which is entered last in the stack will be on the top of the stack thus will be deleted first. [FIFO]

➧ To insert an element we call ENQUEUE operation and to delete an element we call DEQUEUE operation.

➧ It has head and tail. When an element is enqueued, it takes place its place at the tail of the queue and the element dequeued is always the one at the head of the queue.

➧ Q[Q.head, Q.head+1, ..., Q.tail-1]. The Q.tail indexes the next location at which a newly arriving element will be inserted into the queue.

➧ When Q.head=Q.tail, the queue is empty. Initially, Q.head=Q.tail=1. When Q.head=Q.tail+1, the queue is full.

Here is the C++ program for implementing Queue using Linked List.

```cpp
#include <iostream>

template<class T>
class Queue
{
    struct Node
    {
        T data;
        Node* next;
        Node(T val):data(val),next(nullptr) {}
    };
    Node *front, *rear;

  public:
    Queue():front(nullptr),rear(nullptr){}
    ~Queue();
    void enqueue(T val);
    T dequeue();
    Queue(Queue const&)              = delete;
    Queue& operator =(Queue const&)  = delete;

    template <class U>
    friend std::ostream & operator<<(std::ostream & os, const Queue<U> & q)
    {
        q.display(os);
        return os;
    }

  private:
    void display(std::ostream& out = std::cout) const
    {
        for(Node* loop = front; loop != nullptr; loop = loop->next)
        {
              out << loop->data << " ";
        }
    }
};

template<class T>void Queue<T>::enqueue(T val)
{
    Node* node=new Node(val);
    node->next=nullptr;
    if(front==nullptr)
    {
        front=rear=node;
        rear->next=nullptr;
    }
    else
    {
        rear->next=node;
        rear=node;
        rear->next=nullptr;
    }
}

template<class T>T Queue<T>::dequeue()
{
    if(front==nullptr)
        std::cout<<"Empty Queue"<<std::endl;
    else
    {
        Node* node=front;
        front=front->next;
        T tmp=std::move(node->data);
        delete node;
        return tmp;
    }
}
template<class T>Queue<T>::~Queue()
{
    for(Node* next;front!=nullptr;front=next)
    {
        next=front->next;
        delete front;
    }
}

int main()
{
    Queue<int> q1;
    q1.enqueue(5);
    q1.enqueue(8);
    q1.enqueue(1);
    q1.enqueue(30);
    std::cout<<q1<<std::endl;
    std::cout<<q1.dequeue()<<std::endl;
    std::cout<<q1<<std::endl;
    return 0;
}
```

You may also like:
[C++: Selection sort using STL](https://programmercave00.github.io/blog/2017/08/29/C++-Selection-sort-using-STL)

[C++: Implementation of Merge Sort](https://programmercave00.github.io/blog/2017/08/24/C++-Implementation-of-Merge-Sort)

[C++: Insertion Sort using STL (Sorting)](https://programmercave00.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))

[C++: Implementation of Quicksort (Sorting)](https://programmercave00.github.io/blog/2017/07/16/C++-Implementation-of-Quicksort-(Sorting))

[C++: Implementation of Heapsort (Sorting)](https://programmercave00.github.io/blog/2017/07/15/C++-Implementation-of-Heapsort-(Sorting))

[C++: Maximum Priority Queue](https://programmercave00.github.io/blog/2017/09/04/C++-Maximum-Priority-Queue)




