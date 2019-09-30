---
layout: post
title: "Python: Heapsort"
tags:  [Python, Algorithm, Sorting]
date: 2019-09-30
---

In a **max-heap**, the max-heap property is that for every node *i* other than the root,`A[PARENT (i)] ≥ A[i]`

The largest element in a max-heap is stored at the root.

![useful image]({{ site.url }}/assets/pythonheapsort1.png)

The `max_heapify` function, which runs in O(lg n) time, is key to maintaining the max-heap property.

```python
def max_heapify(thelist, lst_size: int, idx: int):
    """
    Building maximum heap at index passed as argument
    """
    largest = idx
    left_child = 2 * idx + 1
    right_child = 2 * idx + 2

    #comparing left child and largest
    if left_child < lst_size and thelist[left_child] > thelist[largest]:
        largest = left_child

    #comparing right child and largest
    if right_child < lst_size and thelist[right_child] > thelist[largest]:
        largest = right_child

    if largest != idx: #if largest is changed
        thelist[idx], thelist[largest] = thelist[largest], thelist[idx] #swapping
        max_heapify(thelist, lst_size, largest)
  ```
  
  Action of `max_heapify`
  
  ![useful image]({{ site.url }}/assets/pythonheapsort2.png)
  
  The `build_max_heap` function, which runs in linear time, produces a max-heap from an unorderd array.
  
  ```python
  def build_max_heap(thelist, lst_size: int):
    """
    Building maximum heap from the list passed as argument
    """
    for curr_idx in range(lst_size // 2 - 1, -1, -1): #loop from higher index to lower index
        max_heapify(thelist, lst_size, curr_idx)
  ```
  
The `heapsort` function, which runs in O(n lg n) time, sorts an array in place.
  
```python
  def heap_sort(thelist):
    """
    Heap sort Algorithm
    """
    if len(thelist) == 0:
        print("Empty list!!")

    elif len(thelist) == 1:
        print("Only one element!!")

    else:
        build_max_heap(thelist, len(thelist))

        for curr_idx in range(len(thelist) -1, 0, -1): #loop from higher index to lower index
            thelist[curr_idx], thelist[0] = thelist[0], thelist[curr_idx] #swapping
            max_heapify(thelist, curr_idx, 0)
```
 
 Operation of `heapsort`
    
 ![useful image]({{ site.url }}/assets/pythonheapsort3.png)
    
 Python Implementation
    
```
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

def max_heapify(thelist, lst_size: int, idx: int):
    """
    Building maximum heap at index passed as argument
    """
    largest = idx
    left_child = 2 * idx + 1
    right_child = 2 * idx + 2

    #comparing left child and largest
    if left_child < lst_size and thelist[left_child] > thelist[largest]:
        largest = left_child

    #comparing right child and largest
    if right_child < lst_size and thelist[right_child] > thelist[largest]:
        largest = right_child

    if largest != idx: #if largest is changed
        thelist[idx], thelist[largest] = thelist[largest], thelist[idx] #swapping
        max_heapify(thelist, lst_size, largest)

def build_max_heap(thelist, lst_size: int):
    """
    Building maximum heap from the list passed as argument
    """
    for curr_idx in range(lst_size // 2 - 1, -1, -1): #loop from higher index to lower index
        max_heapify(thelist, lst_size, curr_idx)

def heap_sort(thelist):
    """
    Heap sort Algorithm
    """
    if len(thelist) == 0:
        print("Empty list!!")

    elif len(thelist) == 1:
        print("Only one element!!")

    else:
        build_max_heap(thelist, len(thelist))

        for curr_idx in range(len(thelist) -1, 0, -1): #loop from higher index to lower index
            thelist[curr_idx], thelist[0] = thelist[0], thelist[curr_idx] #swapping
            max_heapify(thelist, curr_idx, 0)

if __name__ == '__main__':
    input_list = get_input()
    heap_sort(input_list)
    print(*input_list, sep = ", ")
```

The images and content are from *Introduction to Algorithms*.

Output:

![useful image]({{ site.url }}/assets/pythonheapsortout.png)


 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like
[Python: Selection sort](https://programmercave.github.io/blog/2019/09/30/Python-Selection-sort)<br/>
[Python: Quicksort](https://programmercave.github.io/blog/2019/09/30/Python-Quicksort)<br/>
[Python: Merge Sort](https://programmercave.github.io/blog/2019/09/30/Python-Merge-Sort)<br/>
[Python: Insertion Sort](https://programmercave.github.io/blog/2019/09/30/Python-Insertion-Sort)<br/>
[Python: Heapsort](https://programmercave.github.io/blog/2019/09/30/Python-Heapsort)<br/>
[C++: Implementation of Merge Sort](https://programmercave.github.io/blog/2017/08/24/C++-Implementation-of-Merge-Sort)<br/>
[C++: Insertion Sort using STL (Sorting)](https://programmercave.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))<br/>
[C++: Implementation of Quicksort (Sorting)](https://programmercave.github.io/blog/2017/07/16/C++-Implementation-of-Quicksort-(Sorting))<br/>
[C++: Maximum Priority Queue](https://programmercave.github.io/blog/2017/09/04/C++-Maximum-Priority-Queue)<br/>
[C++: Insertion Sort using STL (Sorting)](https://programmercave.github.io/blog/2017/08/20/C++-Insertion-Sort-using-STL-(Sorting))<br/>
[C++: Selection sort using STL](https://programmercave.github.io/blog/2017/08/29/C++-Selection-sort-using-STL)

    
  
  
  
        

