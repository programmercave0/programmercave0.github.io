---
layout: post
title: "C++: Implementation of Quicksort (Sorting)"
tags:  [C++, Algorithm, Sorting]
date: 2017-07-16
---
Quicksort applies Divide and Conquer paradigm.

Divide: Partition the array A[p…r] into two subarrays A[p…q-1] and A[q+1…r] such that each element of A[p...q-1] is
less than or equal to a[q] which is, in turn, less than or equal to each element
of  A[q+1…r]

Conquer: Sort the two subarrays A[p…q-1] and A[q+1…r]by recursive calls
to quicksort

Combine: Because the subarrays are already sorted, no work is needed to combine them the entire array A[p...r] is now sorted

![QuickSort]({{ site.url }}/assets/quickSort.PNG)

```cpp
#include <iostream>

int split(int a[],int start_index,int end_index){
int x=a[end_index];
int i=start_index-1;
for(int j=start_index;j<end_index;j++){
    if(a[j]<=x){
        i++;
      int temp=a[i];
      a[i]=a[j];
      a[j]=temp;//swap(a[i],a[j])
    }
}
int temp=a[i+1];
a[i+1]=a[end_index];
a[end_index]=temp;//swap(a[i+1],a[end_index])

return i+1;
}

void quicksort(int a[],int start_index,int end_index){
if(start_index<end_index){
    int mid_index=split(a,start_index,end_index);
                quicksort(a,start_index,mid_index-1);
                quicksort(a,mid_index+1,end_index);
}
}

int main()
{
  int arr[8]={2,8,7,1,3,5,6,4};
    quicksort(arr,0,7);
    for(int i=0;i<8;i++){
        std::cout<<arr[i]<<" ";
    }
    return 0;
}
```

 Running time of partition is Θ(n)

Worst case running time of algorithm when the partition is unbalanced means partition procedure produces one subarray of n-1 elements and other of 0 elements  that is unbalanced partitioning.Then its running time is 
\Theta(n^2)Θ(n​2​​).

Best case running time is when there is balanced partitioning. Then its running time is 

\Theta(n^2)Θ(n lg n).

The version of partition algorithm I have used above is not original partitioning algorithm. The original partition algorithm was given by C.A.R. Hoare. Use it in your program.

```
 HOARE_PARTITION(A,p,r)
1. x=A[p]
2. i=p-1
3. j=r+1
4. while TRUE
5.      repeat
6.         j=j-1
7.      until
8.      repeat
9.         i=i-1
10.     until A[i]>=x
11.     if i<j
12.        exchange A[i]with A[j]
13     else return j
```

 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like


[C++: Selection sort using STL](https://programmercave0.github.io/blog/2017/08/29/C++-Selection-sort-using-STL)

[C++: Implementation of Merge Sort](https://programmercave0.github.io/blog/2017/08/24/C++-Implementation-of-Merge-Sort)

[C++: Insertion Sort using STL (Sorting)](https://programmercave0.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))

[C++: Implementation of Heapsort (Sorting)](https://programmercave0.github.io/blog/2017/07/15/C++-Implementation-of-Heapsort-(Sorting))

[C++: Maximum Priority Queue](https://programmercave0.github.io/blog/2017/09/04/C++-Maximum-Priority-Queue)



