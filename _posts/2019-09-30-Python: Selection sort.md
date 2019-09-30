---
layout: post
title: "Python: Selection sort"
tags:  [Python, Algorithm, Sorting]
date: 2019-09-30
---

**Selection sort** divides the input list or the array in two parts:
    
    1.Sorted array
    2. Unsorted array

The minimum element from the unsorted array is picked and placed at the end of sorted array in each iteration.

![useful image]({{ site.url }}/assets/SelSort1.png)

Python Implementation

```python
def get_input():
    """
    get input from user
    """
    input_str = input("Enter elements to be sorted: ")
    try:
        elements = [int(e) for e in input_str.split()] #make a list of integers from input string
    except ValueError:
        print("Please enter a list of integers only, seperated by a space!!")
    return elements

def selection_sort(thelist):
    """
    Selection sort Algorithm
    """
    if len(thelist) == 0:
        print("Empty list!!")

    elif len(thelist) == 1:
        print("Only one element!!")

    else:
        for i in range(len(thelist)-1):
            min_idx = i #assigning first index as minimum index

            for j in range(i+1, len(thelist)):
                if thelist[j] < thelist[min_idx]:
                    min_idx = j

            thelist[i], thelist[min_idx] = thelist[min_idx], thelist[i] #swapping


if __name__ == '__main__':
    input_list = get_input()
    selection_sort(input_list)
    print(*input_list, sep = ", ")
```


Output

![useful image]({{ site.url }}/assets/pythonselectionsortout.png)


 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like
[Python: Quicksort](https://programmercave.github.io/blog/2019/09/30/Python-Quicksort)<br/>
[Python: Merge Sort](https://programmercave.github.io/blog/2019/09/30/Python-Merge-Sort)<br/>
[Python: Insertion Sort](https://programmercave.github.io/blog/2019/09/30/Python-Insertion-Sort)<br/>
[Python: Heapsort](https://programmercave.github.io/blog/2019/09/30/Python-Heapsort)<br/>
[C++: Implementation of Merge Sort](https://programmercave.github.io/blog/2017/08/24/C++-Implementation-of-Merge-Sort)<br/>
[C++: Insertion Sort using STL (Sorting)](https://programmercave.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))<br/>
[C++: Implementation of Quicksort (Sorting)](https://programmercave.github.io/blog/2017/07/16/C++-Implementation-of-Quicksort-(Sorting))<br/>
[C++: Implementation of Heapsort (Sorting)](https://programmercave.github.io/blog/2017/07/15/C++-Implementation-of-Heapsort-(Sorting))<br/>
[C++: Maximum Priority Queue](https://programmercave.github.io/blog/2017/09/04/C++-Maximum-Priority-Queue)<br/>
[C++: Insertion Sort using STL (Sorting)](https://programmercave.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))<br/>
[C++: Selection sort using STL](https://programmercave.github.io/blog/2017/08/29/C++-Selection-sort-using-STL)



