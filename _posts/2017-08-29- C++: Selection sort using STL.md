---
layout: post
title: "C++: Selection sort using STL"
tags:  [C++, Algorithm, Sorting]
date: 2017-08-29
---

Selection sort is an sorting algorithm. It has O(n<sup>2</sup>) time complexity. It has performance advantages over more complicated algorithms in certain situations, particularly where auxiliary memory is limited.

It divides the input list or the array in two parts:
1. Sorted array
2. Unsorted array

The minimum element from the unsorted array is picked and placed at the end of sorted array in each iteration.

![Selection Sort]({{ site.url }}/assets/SelSort1.png)

C++ Implementation

```cpp
#include <iostream>

void swap(int *num1, int *num2)
{
  int temp   = *num2;
  *num2      = *num1;
  *num1      =  temp;
}

void selectionSort(int array[], int size)
{
  int start_index = 0;
  while(start_index < size)
  {
    int min_index = start_index;
    for(int i = start_index+1; i < size; i++)
    {
      if(array[i] < array[min_index])
      {
        min_index = i;
      }
    }
    swap(&array[start_index], &array[min_index]);
    start_index++;
  }
}

void print(int array[], int size)
{
  for(int i = 0; i < size; i++)
  {
    std::cout << array[i] << " ";
  }
  std::cout << '\n';
}

int main()
{
  int arr[] = {5, 3, 12, 2, 8};
  int size = sizeof(arr)/sizeof(int);
  std::cout << "Unsorted Array: ";
  print(arr, size);
  selectionSort(arr, size);
  std::cout << "Sorted Array: ";
  print(arr, size);
}
```

Here we are only sorting integer values. Now to sort character or string values we will write a code using Template. 

```cpp
#include <iostream>
#include <vector>

template<typename T>
void selectionSort(std::vector<T>& array)
{
  typedef typename std::vector<T>::iterator Itr;
  Itr itr_begin = array.begin();
  while(itr_begin != array.end())
  {
    Itr itr_min = itr_begin;
    for(Itr i = itr_begin + 1; i != array.end(); i++)
    {
      if(*i < *itr_min)
      {
        itr_min = i;
      }
    }
    std::iter_swap(itr_begin, itr_min);
    itr_begin++;
  }
}

template<typename T>
void print(const std::vector<T>& array)
{
  for(auto itr = array.begin(); itr != array.end(); itr++)
  {
    std::cout << *itr << " ";
  }
  std::cout << '\n';
}

int main()
{
  std::vector<int> v({5, 3, 12, 2, 8});
  std::cout << "Original Array :";
  print(v);
  selectionSort(v);
  std::cout <<"Sorted Array :";
  print(v);
  std::cout << '\n';

  std::vector<char> c({'t', 'q', 'a', 'r', 'p'});
  std::cout << "Original Array :";
  print(c);
  selectionSort(c);
  std::cout <<"Sorted Array :";
  print(c);
  std::cout << '\n';

  std::vector<std::string> str({"code", "live", "love", "sing", "create"});
  std::cout << "Original Array :";
  print(str);
  selectionSort(str);
  std::cout <<"Sorted Array :";
  print(str);
  std::cout << '\n';

}

```

Here we have sorted character as well as string values. We have used `std::vector` to store the values instead of array. We have used Standard Template Library functions like std::iter_swap. We can also use `std::swap` instead of `std::iter_swap`.

`std::swap` swap the values where as `std::iter_swap` swap the iterators.

Here we are bound to use `std::vector` if we want to use any container then we will use Forward Iterator.

Here is the code

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>

template <typename ForwardItr>
void selectionSort(ForwardItr first, ForwardItr last)
{
  while(first != last)
  {
    ForwardItr min = std::min_element(first, last);
      std::iter_swap(min, first);
    ++first;
  }
}

template <typename Itr>
void print(Itr first, Itr last)
{
  while(first != last)
  {
    std::cout << *first <<" ";
    ++first;
  }
  std::cout << "\n";
}

int main()
{
  std::vector<int> v({5, 3, 12, 2, 8});
  std::cout << "Original Array :";
  print(v.begin(), v.end());
  selectionSort(v.begin(), v.end());
  std::cout <<"Sorted Array :";
  print(v.begin(), v.end());
  std::cout << '\n';

  std::vector<char> c({'t', 'q', 'a', 'r', 'p'});
  std::cout << "Original Array :";
  print(c.begin(), c.end());
  selectionSort(c.begin(), c.end());
  std::cout <<"Sorted Array :";
  print(c.begin(), c.end());
  std::cout << '\n';

  std::vector<std::string> str({"code", "live", "love", "sing", "create"});
  std::cout << "Original Array :";
  print(str.begin(), str.end());
  selectionSort(str.begin(), str.end());
  std::cout <<"Sorted Array :";
  print(str.begin(), str.end());
  std::cout << '\n';

}
```

Output

![Output]({{ site.url }}/assets/SelSortOut.png)

 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like


[C++: Implementation of Merge Sort](https://programmercave0.github.io/blog/2017/08/24/C++-Implementation-of-Merge-Sort)<br/>
[C++: Insertion Sort using STL (Sorting)](https://programmercave0.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))<br/>
[C++: Implementation of Quicksort (Sorting)](https://programmercave0.github.io/blog/2017/07/16/C++-Implementation-of-Quicksort-(Sorting))<br/>
[C++: Implementation of Heapsort (Sorting)](https://programmercave0.github.io/blog/2017/07/15/C++-Implementation-of-Heapsort-(Sorting))<br/>
[C++: Maximum Priority Queue](https://programmercave0.github.io/blog/2017/09/04/C++-Maximum-Priority-Queue)<br/>
[C++: Insertion Sort using STL (Sorting)](https://programmercave0.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))<br/>
[Python: Selection sort](https://programmercave0.github.io/blog/2019/09/30/Python-Selection-sort)<br/>
[Python: Quicksort](https://programmercave0.github.io/blog/2019/09/30/Python-Quicksort)<br/>
[Python: Merge Sort](https://programmercave0.github.io/blog/2019/09/30/Python-Merge-Sort)<br/>
[Python: Insertion Sort](https://programmercave0.github.io/blog/2019/09/30/Python-Insertion-Sort)<br/>
[Python: Heapsort](https://programmercave0.github.io/blog/2019/09/30/Python-Heapsort)<br/>


