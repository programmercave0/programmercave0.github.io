---
layout: post
title: "C++: Insertion Sort using STL (Sorting)"
tags:  [C++, Algorithm, Sorting]
date: 2017-08-20
---

An insertion sort is generally described simply as: read each element in turn, and insert it in the right position among the previous read (and sorted) elements.

It is much less efficient on large lists than more advanced algorithms such as quicksort, heapsort, or merge sort.

However, insertion sort provides several advantages:

➧ Simple implementation
➧ Efficient for (quite) small data sets, much like other quadratic sorting algorithms. [[wikipedia](https://en.wikipedia.org/wiki/Insertion_sort)]

![Insertion Sort]({{ site.url }}/assets/InsertionSort.png)

C++ Implementation

```cpp
#include<iostream>
#include<vector>
#include<algorithm>

void insertionSort(std::vector<int>& vec){
 for(auto it = vec.begin(); it != vec.end(); it++)
 {
   // Search
   auto const insertion_point = std::upper_bound(vec.begin(), it, *it);

   //insert
   std::rotate(insertion_point, it, it+1);
 }
}

void print(std::vector<int> const& vec){
 for( int x : vec)
  std::cout << x << " ";
std::cout << '\n';
}

int main(){
 std::vector<int> arr = {2, 1, 5, 3, 7, 5, 4, 6};
 insertionSort(arr);
 print(arr);
}
```

Here we have used `std::upper_bound` and `std::rotate`  

`std::upper_bound` 

Returns an iterator pointing to the first element in the range [first, last) that is greater than value.

`std::rotate`

It rotates the order of the elements in the range [first,last], in such a way that the element pointed by middle becomes the new first element.

Simple implementation

```cpp
#include<iostream>
#include<vector>

void insertionSort(std::vector<int>& vec)
{
for(unsigned j = 1; j < vec.size(); j++)
 {
  int key = vec[j];
  int i = j-1;

  while(i >= 0 && vec[i] > key)
  {
    vec[i+1] = vec[i];
    i--;
  }
  vec[i+1] = key;
 }
}

void print(std::vector<int> vec){
 for(unsigned i = 0; i < vec.size(); i++){
  std::cout << vec[i] << " ";
  }
}

int main(){
std::vector<int> arr = {2, 1, 5, 3, 7, 5, 4, 6};
insertionSort(arr);
print(arr);
std::cout<<std::endl;
}
```

 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like

[C++: Selection sort using STL](https://programmercave.github.io/blog/2017/08/29/C++-Selection-sort-using-STL)

[C++: Implementation of Merge Sort](https://programmercave.github.io/blog/2017/08/24/C++-Implementation-of-Merge-Sort)

[C++: Insertion Sort using STL (Sorting)](https://programmercave.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))

[C++: Implementation of Quicksort (Sorting)](https://programmercave.github.io/blog/2017/07/16/C++-Implementation-of-Quicksort-(Sorting))

[C++: Implementation of Heapsort (Sorting)](https://programmercave.github.io/blog/2017/07/15/C++-Implementation-of-Heapsort-(Sorting))

[C++: Maximum Priority Queue](https://programmercave.github.io/blog/2017/09/04/C++-Maximum-Priority-Queue)
