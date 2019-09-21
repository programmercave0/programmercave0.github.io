---
layout: post
title: "C++: Collatz Conjecture Program "
tags:  C++, Algorithm, Maths
date: 2018-04-17
---

The **Collatz conjecture** is a conjecture in mathematics that concerns a sequence defined as follows: start with any positive integer n. Then each term is obtained from the previous term as follows: if the previous term is even, the next term is one half the previous term. Otherwise, the next term is 3 times the previous term plus 1. The conjecture is that no matter what value of n, the sequence will always reach 1. [wikipedia](https://en.wikipedia.org/wiki/Collatz_conjecture)

![Collatz Conjecture]({{ site.url }}/assets/CollatzFormula.gif)

C++ implementation of Collatz conjecture

```cpp
#include <iostream>
#include <cassert> //assert()

int main()
{
    int n;
    std::cout << "Enter number (greater than 1)\n";
    std::cin >> n;
    assert(n > 1);
    std::cout << "Series is :\n";
    std::cout << n << " ";
    while(n != 1)
    {
        if (n % 2 == 0)
        {
            n = n / 2;
            std::cout << n << " ";
        }
        else
        {
            n = (3 * n) + 1;
            std::cout  << n << " ";
        }
    }
    std::cout << '\n';

}
```
