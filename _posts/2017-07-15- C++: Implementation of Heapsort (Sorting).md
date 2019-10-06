---
layout: post
title: "C++: Implementation of Heapsort (Sorting)"
tags:  [C++, Algorithm, Sorting]
date: 2017-07-15
---

`max_heapify()` function: The running time of this procedure is 
O(lg n) and it helps to maintain the max-heap property. Max-heap property means every node other than root node must be smaller than or equal to its parent.

`build_max_heap()` function: Its running time is O(n lg n)

Result after execution of `build_max_heap()` function is

![Heapsort]({{ site.url }}/assets/Heapsort1.PNG)

`heap_sort` function: Its running time is O(n lg n).

Result after excution of `heap_sort()` is

![Heapsort]({{ site.url }}/assets/Heapsort2.PNG)

C++ Implementation

```cpp
#include <iostream>

 void max_heapify(int arr[], int i, int si)
 {
   int largest, l = (2*i) + 1, r = l + 1;


   if(l < si && arr[l] > arr[i])
      largest = l;
   else
    largest = i;

   if(r < si && arr[r] > arr[largest])
    largest = r;

   if(largest != i)
   {
      int temp = arr[i];
      arr[i] = arr[largest];
      arr[largest] = temp;
      max_heapify(arr, largest, si);
   }
  }

 void build_max_heap(int arr[], int si)
 {
   for(int i = (si/2) - 1; i >=0; i--)
    max_heapify(arr, i, si);
 }

 void heap_sort(int arr[], int si)
 {
   build_max_heap(arr, si);
   int sz = si;
   for(int i = si - 1;i > 0; i--)
   {
      int temp = arr[i];
      arr[i]   = arr[0];
      arr[0]   = temp;
      sz--;
      max_heapify(arr, 0, sz);
    }
 }

 int main()
 {
    int a[10] = {4, 1, 3, 2, 16, 9, 10, 14, 8, 7};
    int s = sizeof(a)/sizeof(int);
    heap_sort(a, s);
    for(int i = 0;i < s; i++){
    std::cout << a[i] << " ";
 }
    return 0;
}
```

 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like
[C++: Selection sort using STL](https://programmercave0.github.io/blog/2017/08/29/C++-Selection-sort-using-STL)

[C++: Implementation of Merge Sort](https://programmercave0.github.io/blog/2017/08/24/C++-Implementation-of-Merge-Sort)

[C++: Insertion Sort using STL (Sorting)](https://programmercave0.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))

[C++: Implementation of Quicksort (Sorting)](https://programmercave0.github.io/blog/2017/07/16/C++-Implementation-of-Quicksort-(Sorting))

[C++: Maximum Priority Queue](https://programmercave0.github.io/blog/2017/09/04/C++-Maximum-Priority-Queue)
