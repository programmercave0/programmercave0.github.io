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


