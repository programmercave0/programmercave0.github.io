---
layout: post
title: "C++: Sorting Elements according to Frequency"
tags:  [C++, Algorithm, Sorting, Misc]
date: 2017-08-19
---

We are going to write a program to sort the elements of an array by Frequency. If two elements have the same frequency of occurrence, then they are sorted in increasing order.

```
Input: { 2, 3, 2, 4, 5, 12, 12, 3, 4, 3 }
Output: {3, 3, 3, 2, 2, 4, 4, 12, 12, 5}
```
C++ Implementation

```cpp
#include <iostream>
 #include <vector>
 #include <map>
 #include <algorithm>

 struct greater
 {
    template<class T>
    bool operator()(T const &a, T const &b) const { return a.second > b.second; }
 };

 template<class T>
 void printSortByFreq(std::vector<T>& v)
 {
   std::map<T, size_t> count;

   for (T x : v){
       count[x]++;
   }

   std::vector<std::pair<T, size_t>> mapcopy;

   for(auto itr = count.begin(); itr != count.end(); itr++)
   {
     mapcopy.push_back(*itr);
   }

   std::sort(mapcopy.begin(), mapcopy.end(), greater());

   std::cout << "Elements sorted according to Frequency \n";

   auto itr = mapcopy.begin();
   while(itr != mapcopy.end())
   {
     for(size_t i = 0; i < itr->second; i++)
     {
       std::cout << itr->first <<" ";
     }
   itr++;
   }
   std::cout<<"\n";
 }

 int main()
 {
   std::vector<int> vec = {2, 3, 2, 4, 5, 12, 12, 3, 4, 3};
   printSortByFreq(vec);
   std::vector<char> c = { 'e', 't', 'r', 'e', 't', 't', 'f'};
   printSortByFreq(c);
 return 0;
}
```
