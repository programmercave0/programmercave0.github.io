---
layout: post
title: "C++: Implementation of Stack using Array (Data Structure)"
tags:  [C++, Algorithm, Stack, Data Structure]
date: 2017-07-20
---

Stack:

➧ It is a dynamic set. What are dynamic sets? Sets which can be manipulated by algorithms, which can grow, shrink or change over time.

➧ It implements last-in,first-out means element which is entered last in the stack will be on the top of the stack thus will be deleted first. [LIFO]

➧ To insert an element we call PUSH operation and to delete an element we call POP operation .

➧ S[0,...,S.top] here S[0] shows element at the bottom and S[S.top] shows element at the top.

➧ S.top= -1 means stack contains no element and is empty.

![Stack]({{ site.url }}/assets/stack2.PNG)

C++ Implementation

```cpp
#include <iostream>

#define max 1000
class Stack{
  int top;

 public:
   int a[max];
     Stack(){
      top=-1;
      }
      bool stack_empty();
      void push(int x);
      int pop();
      void display();
};

bool Stack::stack_empty(){
if(top==-1)
    return true;
else
    return false;
}

void Stack::push(int x){
  int s=max-1;
if(top<s){
top=top+1;
a[top]=x;
}
else
    std::cout<<"overflow"<<"\n";
}

int Stack::pop(){
if (stack_empty()==true)
    std::cout<<"Underflow"<<"\n";
else{
    --top;
    return a[top+1];
}
}

void Stack::display(){
for(int i=0;i<=top;i++){
    std::cout<<a[i]<<" ";
}
}

int main()
{
    Stack stack1;
    stack1.push(15);
    stack1.push(6);
    stack1.push(2);
    stack1.push(9);
    stack1.push(3);
    stack1.display();
    std::cout<<"\n";
    std::cout<<stack1.pop()<<"\n";
    stack1.display();
    return 0;
}
```


