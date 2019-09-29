---
layout: post
title: "C++: Stack implementation using LinkedList (Data Structure)"
tags:  [C++, Algorithm, Stack, Linked List, Data Structures]
date: 2017-07-31
---

Stack:

➧ It is a dynamic set. What are dynamic sets? Sets which can be manipulated by algorithms, which can grow, shrink or change over time.

➧ It implements last-in,first-out means element which is entered last in the stack will be on the top of the stack thus will be deleted first. [LIFO]

➧ To insert an element we call PUSH operation and to delete an element we call POP operation .

➧ S[0,...,S.top] here S[0] shows element at the bottom and S[S.top] shows element at the top.

➧ S.top= -1 means stack contains no element and is empty.

For `push()` and `pop()` function we have to use `insert()` and `deleteNode()` function from Linked List program.

C++ Implementation

```cpp
#include <iostream>

template<class T>
class Stack
{
    struct Node
    {
        T data;
        Node *next;
        Node(T val):data(val),next(nullptr) {}
    };
    Node *head; int top;

 public:
   Stack()
   {
        top=-1;
        head=nullptr;
    }
   ~Stack();
    bool stack_empty();
    void push(T);
    T pop();

   void display(std::ostream& out = std::cout) const
   {
        Node *node=head;
        while(node!=nullptr)
        {
            out<<node->data<<" ";
            node=node->next;
        }
    }
};

template<class T>
bool Stack<T>::stack_empty()
{
    if(top==-1)
        return true;
    else
        return false;
}

template<class T>
void Stack<T>::push(T val)
{
    top++;
    Node *node=new Node(val);
    node->next = head;
    head=node;
}

template<class T>
T Stack<T>::pop()
{
    if(stack_empty()==true)
    std::cout<<"Underflow"<<std::endl;
    else
    {
        top--;
        Node *node=head;
        head = node->next;
        T tmp = std::move(node->data);
        delete node;
        return tmp;
    }
}

template<class U>
std::ostream & operator <<(std::ostream& os, const Stack<U>& s)
{
    s.display(os);
    return os;
}

template<class T>
Stack<T>::~Stack()
{
    Node *tmp=nullptr;
    while(head)
    {
        tmp=head;
        head=head->next;
        delete tmp;
    }
    head=nullptr;
}

int main()
{
    Stack<int> stack1;
    stack1.push(3);
    stack1.push(4);
    stack1.push(11);
    stack1.push(30);
    std::cout<<stack1<<std::endl;
    std::cout<<stack1.pop()<<std::endl;
    return 0;
}
```

Remember to use `delete` to delete a node to avoid memory leak.

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
[C++: Doubly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/28/C++-Doubly-Linked-List-using-Template-(Data-Structure))
[C++: Singly Linked List using Template (Data Structure)](https://programmercave.github.io/blog/2017/07/27/C++-Singly-Linked-List-using-Template-(Data-Structure))






