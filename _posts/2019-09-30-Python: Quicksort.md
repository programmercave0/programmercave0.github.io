---
layout: post
title: "Python: Quicksort"
tags:  [Python, Algorithm, Sorting]
date: 2019-09-30
---

**Quicksort** is a sorting algorithm whose worst-case running time is &theta;(n<sup>2</sup>) on an input array of n numbers. In spite of this slow worst-case running time, quicksort is often the best practical choice for sorting because it is remarkably efficient on the average: its expected running time is &theta;(n lg n), and the constant factors hidden in the &theta;(n lg n) notation are quite small.

```python
def partition(thelist, start_idx: int, end_idx: int):
    """
    partition list according to pivot and return index of pivot
    """
    pivot = thelist[end_idx] #select last element as the pivot
    idx = start_idx - 1

    for curr_idx in range(start_idx, end_idx):
        if thelist[curr_idx] <= pivot:
            idx += 1 #increment
            thelist[idx], thelist[curr_idx] = thelist[curr_idx], thelist[idx] #swapping

    thelist[idx+1], thelist[end_idx] = thelist[end_idx], thelist[idx+1] #swapping
    return idx+1 #returning pivot index
```

Operation of `partition`

![useful image]({{ site.url }}/assets/pythonquicksort1.png)

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

def partition(thelist, start_idx: int, end_idx: int):
    """
    partition list according to pivot and return index of pivot
    """
    pivot = thelist[end_idx] #select last element as the pivot
    idx = start_idx - 1

    for curr_idx in range(start_idx, end_idx):
        if thelist[curr_idx] <= pivot:
            idx += 1 #increment
            thelist[idx], thelist[curr_idx] = thelist[curr_idx], thelist[idx] #swapping

    thelist[idx+1], thelist[end_idx] = thelist[end_idx], thelist[idx+1] #swapping
    return idx+1 #returning pivot index

def quick_sort(thelist, start_idx: int, end_idx: int):
    """
    Quick Sort Algorithm
    """
    if len(thelist) == 0:
        print("Empty list!!")

    elif len(thelist) == 1:
        print("Only one element!!")

    elif start_idx < end_idx:
        pivot_idx = partition(thelist, start_idx, end_idx) #get pivot index
        quick_sort(thelist, start_idx, pivot_idx - 1) #apply algorithm for smaller list
        quick_sort(thelist, pivot_idx + 1, end_idx) #apply algorithm for smaller list

if __name__ == '__main__':
    input_list = get_input()
    quick_sort(input_list, 0, len(input_list) - 1)
    print(*input_list, sep = ", ")
```

Output

![useful image]({{ site.url }}/assets/pythonquicksortout.png)


 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like
[Python: Selection sort](https://programmercave.github.io/blog/2019/09/30/Python-Selection-sort)<br/>
[Python: Merge Sort](https://programmercave.github.io/blog/2019/09/30/Python-Merge-Sort)<br/>
[Python: Insertion Sort](https://programmercave.github.io/blog/2019/09/30/Python-Insertion-Sort)<br/>
[Python: Heapsort](https://programmercave.github.io/blog/2019/09/30/Python----
layout: post
title: "Python: Quicksort"
tags:  [Python, Algorithm, Sorting]
date: 2019-09-30
---

**Quicksort** is a sorting algorithm whose worst-case running time is &theta;(n<sup>2</sup>) on an input array of n numbers. In spite of this slow worst-case running time, quicksort is often the best practical choice for sorting because it is remarkably efficient on the average: its expected running time is &theta;(n lg n), and the constant factors hidden in the &theta;(n lg n) notation are quite small.

```python
def partition(thelist, start_idx: int, end_idx: int):
    """
    partition list according to pivot and return index of pivot
    """
    pivot = thelist[end_idx] #select last element as the pivot
    idx = start_idx - 1

    for curr_idx in range(start_idx, end_idx):
        if thelist[curr_idx] <= pivot:
            idx += 1 #increment
            thelist[idx], thelist[curr_idx] = thelist[curr_idx], thelist[idx] #swapping

    thelist[idx+1], thelist[end_idx] = thelist[end_idx], thelist[idx+1] #swapping
    return idx+1 #returning pivot index
```

Operation of `partition`

![useful image]({{ site.url }}/assets/pythonquicksort1.png)

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

def partition(thelist, start_idx: int, end_idx: int):
    """
    partition list according to pivot and return index of pivot
    """
    pivot = thelist[end_idx] #select last element as the pivot
    idx = start_idx - 1

    for curr_idx in range(start_idx, end_idx):
        if thelist[curr_idx] <= pivot:
            idx += 1 #increment
            thelist[idx], thelist[curr_idx] = thelist[curr_idx], thelist[idx] #swapping

    thelist[idx+1], thelist[end_idx] = thelist[end_idx], thelist[idx+1] #swapping
    return idx+1 #returning pivot index

def quick_sort(thelist, start_idx: int, end_idx: int):
    """
    Quick Sort Algorithm
    """
    if len(thelist) == 0:
        print("Empty list!!")

    elif len(thelist) == 1:
        print("Only one element!!")

    elif start_idx < end_idx:
        pivot_idx = partition(thelist, start_idx, end_idx) #get pivot index
        quick_sort(thelist, start_idx, pivot_idx - 1) #apply algorithm for smaller list
        quick_sort(thelist, pivot_idx + 1, end_idx) #apply algorithm for smaller list

if __name__ == '__main__':
    input_list = get_input()
    quick_sort(input_list, 0, len(input_list) - 1)
    print(*input_list, sep = ", ")
```

Output

![useful image]({{ site.url }}/assets/pythonquicksortout.png)


 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like
[Python: Selection sort](https://programmercave0.github.io/blog/2019/09/30/Python-Selection-sort)<br/>
[Python: Merge Sort](https://programmercave0.github.io/blog/2019/09/30/Python-Merge-Sort)<br/>
[Python: Insertion Sort](https://programmercave0.github.io/blog/2019/09/30/Python-Insertion-Sort)<br/>
[Python: Heapsort](https://programmercave0.github.io/blog/2019/09/30/Python-Heapsort)<br/>
[C++: Implementation of Merge Sort](https://programmercave0.github.io/blog/2017/08/24/C++-Implementation-of-Merge-Sort)<br/>
[C++: Insertion Sort using STL (Sorting)](https://programmercave0.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))<br/>
[C++: Implementation of Quicksort (Sorting)](https://programmercave0.github.io/blog/2017/07/16/C++-Implementation-of-Quicksort-(Sorting))<br/>
[C++: Implementation of Heapsort (Sorting)](https://programmercave0.github.io/blog/2017/07/15/C++-Implementation-of-Heapsort-(Sorting))<br/>
[C++: Maximum Priority Queue](https://programmercave0.github.io/blog/2017/09/04/C++-Maximum-Priority-Queue)<br/>
[C++: Insertion Sort using STL (Sorting)](https://programmercave0.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))<br/>
[C++: Selection sort using STL](https://programmercave0.github.io/blog/2017/08/29/C++-Selection-sort-using-STL)










---
layout: post
title: "Python: Quicksort"
tags:  [Python, Algorithm, Sorting]
date: 2019-09-30
---

**Quicksort** is a sorting algorithm whose worst-case running time is &theta;(n<sup>2</sup>) on an input array of n numbers. In spite of this slow worst-case running time, quicksort is often the best practical choice for sorting because it is remarkably efficient on the average: its expected running time is &theta;(n lg n), and the constant factors hidden in the &theta;(n lg n) notation are quite small.

```python
def partition(thelist, start_idx: int, end_idx: int):
    """
    partition list according to pivot and return index of pivot
    """
    pivot = thelist[end_idx] #select last element as the pivot
    idx = start_idx - 1

    for curr_idx in range(start_idx, end_idx):
        if thelist[curr_idx] <= pivot:
            idx += 1 #increment
            thelist[idx], thelist[curr_idx] = thelist[curr_idx], thelist[idx] #swapping

    thelist[idx+1], thelist[end_idx] = thelist[end_idx], thelist[idx+1] #swapping
    return idx+1 #returning pivot index
```

Operation of `partition`

![useful image]({{ site.url }}/assets/pythonquicksort1.png)

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

def partition(thelist, start_idx: int, end_idx: int):
    """
    partition list according to pivot and return index of pivot
    """
    pivot = thelist[end_idx] #select last element as the pivot
    idx = start_idx - 1

    for curr_idx in range(start_idx, end_idx):
        if thelist[curr_idx] <= pivot:
            idx += 1 #increment
            thelist[idx], thelist[curr_idx] = thelist[curr_idx], thelist[idx] #swapping

    thelist[idx+1], thelist[end_idx] = thelist[end_idx], thelist[idx+1] #swapping
    return idx+1 #returning pivot index

def quick_sort(thelist, start_idx: int, end_idx: int):
    """
    Quick Sort Algorithm
    """
    if len(thelist) == 0:
        print("Empty list!!")

    elif len(thelist) == 1:
        print("Only one element!!")

    elif start_idx < end_idx:
        pivot_idx = partition(thelist, start_idx, end_idx) #get pivot index
        quick_sort(thelist, start_idx, pivot_idx - 1) #apply algorithm for smaller list
        quick_sort(thelist, pivot_idx + 1, end_idx) #apply algorithm for smaller list

if __name__ == '__main__':
    input_list = get_input()
    quick_sort(input_list, 0, len(input_list) - 1)
    print(*input_list, sep = ", ")
```

Output

![useful image]({{ site.url }}/assets/pythonquicksortout.png)


 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like
[Python: Selection sort](https://programmercave0.github.io/blog/2019/09/30/Python-Selection-sort)<br/>
[Python: Merge Sort](https://programmercave0.github.io/blog/2019/09/30/Python-Merge-Sort)<br/>
[Python: Insertion Sort](https://programmercave0.github.io/blog/2019/09/30/Python-Insertion-Sort)<br/>
[Python: Heapsort](https://programmercave0.github.io/blog/2019/09/30/Python-Heapsort)<br/>
[C++: Implementation of Merge Sort](https://programmercave0.github.io/blog/2017/08/24/C++-Implementation-of-Merge-Sort)<br/>
[C++: Insertion Sort using STL (Sorting)](https://programmercave0.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))<br/>
[C++: Implementation of Quicksort (Sorting)](https://programmercave0.github.io/blog/2017/07/16/C++-Implementation-of-Quicksort-(Sorting))<br/>
[C++: Implementation of Heapsort (Sorting)](https://programmercave0.github.io/blog/2017/07/15/C++-Implementation-of-Heapsort-(Sorting))<br/>
[C++: Maximum Priority Queue](https://programmercave0.github.io/blog/2017/09/04/C++-Maximum-Priority-Queue)<br/>
[C++: Insertion Sort using STL (Sorting)](https://programmercave0.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))<br/>
[C++: Selection sort using STL](https://programmercave0.github.io/blog/2017/08/29/C++-Selection-sort-using-STL)













