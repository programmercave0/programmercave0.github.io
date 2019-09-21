---
layout: post
title: "C++: Find Prime Factors of a Number"
tags:  [C++, Algorithm, Maths]
date: 2018-04-12
---

**Prime Factorization** is finding which prime numbers multiply together to make the original number.[mathsisfun](https://www.mathsisfun.com/prime-factorization.html)

```cpp
#include <iostream>
#include <vector>

std::vector<int> find_prime_factors(int n)
{
    //int val = n;
    std::vector<int> result;
    for (int i = 2; i <= n; i++)
    {
        while (n % i == 0)
        {
            result.push_back(i);
            n = n/i;
        }
    }
    return result;
}

int main()
{
    int n;
    std::cout << "Enter number\n";
    std::cin >> n;
    std::vector<int> prime_factors;
    prime_factors = find_prime_factors(n);
    std::cout << "Prime Factors of " << n << ":\n";
    for (int i = 0; i < prime_factors.size(); i++)
    {
        std::cout << prime_factors[i] << " ";
    }
    std::cout << "\n";
}
```

Output
![Output]({{ site.url }}/assets/PrimeFactorsOut.png)
