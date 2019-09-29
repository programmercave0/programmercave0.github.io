---
layout: post
title: "C++: Remove all adjacent duplicates from String using Stack"
tags:  [C++, Algorithm, Data Structures, Stack]
date: 2018-02-12
---

Given, a string we have to remove the adjacent duplicates. 

For eg:

>Input  :  careermonk
>Output :  camonk 

>Input  :  mississippi
>Output :  m

We will a character on the stack and check the character on the stack with next character from the string. If the character matches we will pop the character from stack and if they do not match we will push the next character on the stack and proceed.

Here is the C++ implementation

```cpp
#include <iostream>
#include <string>

#define max 1000

class Stack
{
  int top;
public:
  char stk[max];
  Stack()
  {
    top = -1;
  }
  bool isStackEmpty();
  void push(char);
  char pop();
  std::string modifyString(std::string);
private:
  void removeAdjacentDuplicate(std::string);
};

bool Stack::isStackEmpty()
{
   if (top == -1)
      return true;
   else
      return false;
}

void Stack::push(char val)
{
   int s = max - 1;
   if (top < s)
   {
     top = top + 1;
     stk[top] = val;
   }
   else
    std::cerr << "Stack Overflow \n";
}

char Stack::pop()
{
   if (isStackEmpty() == true)
       std::cerr << "Stack Underflow \n";
   else
   {
      --top;
      return stk[top + 1];
   }
}

void Stack::removeAdjacentDuplicate(std::string str)
{
   int len = str.length();
   for (int i = 0; i < len; i++)
   {
      if (isStackEmpty() == true)
      {
         push(str[i]);
      }
      else if (str[i] == stk[top])
      {
         int discard = pop();
      }
      else
      {
         push(str[i]);
      }
   }
}

std::string Stack::modifyString(std::string str)
{
   removeAdjacentDuplicate(str);
   str.resize(top + 1);
   for (int i = 0; i <= top; i++)
   {
     str[i] = stk[i];
   }
   str[top + 1] = '\0';
   return str;
}

int main()
{
   Stack stack1;
   std::string str = "careermonk";
   std::cout << "Orignal string : " << str << "\n";
   str = stack1.modifyString(str);
   std::cout << "New string     : " << str << "\n";

   Stack stack2;
   std::string str2 = "mississippi";
   std::cout << "Original string : " << str2 << "\n";
   str2 = stack2.modifyString(str2);
   std::cout << "New string      : " << str2 << "\n";
}
```

Another Implementation

```cpp
#include <iostream>
#include <string>
#include <algorithm>

std::string & removeDuplicate(std::string& str)
{
  int length = str.length();
  for(unsigned int i = 0; i < length; i++)
  {
    char currChar = str[i]; //holds current character
    for(unsigned int j = i+1; j < length; j++)
    {
      if(currChar == str[j])
        str.erase (std::remove(str.begin()+j, str.end(), str[j]), str.end());
    }
  }
  return str;
}

int main()
{
  std::string str;
  std::cout << "Enter string \n";
  std::getline(std::cin, str);
  str = removeDuplicate(str);
  std::cout <<"New String is " << str << "\n";
}
```

 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like

[C++: Stack implementation using LinkedList (Data Structure)](https://programmercave.github.io/blog/2017/07/31/C++-Stack-implementation-using-LinkedList-(Data-Structure))

[C++: Implementation of Stack using Array (Data Structure)](https://programmercave.github.io/blog/2017/07/20/C++-Implementation-of-Stack-using-Array-(Data-Structure))
