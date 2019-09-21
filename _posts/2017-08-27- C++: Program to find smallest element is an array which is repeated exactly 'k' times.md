---
layout: post
title: "C++: Program to find smallest element is an array which is repeated exactly 'k' times"
tags:  [C++, Algorithm, Misc]
date: 2017-08-27
---

This is a problem from [GeeksForGeeks](https://www.geeksforgeeks.org/smallest-element-array-repeated-exactly-k-times/). 

The problem says we have an array of elements . We have to find the smallest element which is repeated exactly *k* times.

![Image]({{ site.url }}/assets/kTimes.png)

Here we want to find the smallest element in the vector which is repeated exactly 2 times.
Here *r* is repeated 2 times, *t* is repeated 1 time, *q* is repeated 2 times, *u* is repeated 1 time and *s* is repeated 1 time.

C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <map>

template<class T>
void findElement(std::vector<T>& vec, int k)
{
  std::map<T, int> count;
  for(const T& x : vec)
  {
    count[x]++;
  }

  typename std::map<T, int>::iterator itr;
  for(itr = count.begin(); itr != count.end(); itr++)
  {
    if(itr->second == k)
    {
      std::cout << itr->first <<'\n';
      return;
    }
  }
  std::cerr << "No such element \n ";
}

int main()
{
  std::vector<char> v;
  v.push_back('r');
  v.push_back('t');
  v.push_back('q');
  v.push_back('r');
  v.push_back('u');
  v.push_back('q');
  v.push_back('s');
  int k;
  std::cout << " Enter the number of repetitions you want : ";
  std::cin >> k;
  std::cout << "The smallest element that has " << k <<" reptition is : ";
  findElement(v, k);
}
```

 Here we have use `std::map` which will count the occurrence of the elements.
 
 ```cpp
 for(const T& x : vec)
 {
    count[x]++;
 }
 ```

Here we have passed the reference of the elements stored in vector and then count maps these elements with their number of occurrence.



