---
layout: post
title: "Python: Insertion Sort"
tags:  [Python, Algorithm, Sorting]
date: 2019-09-30
---

**Insertion sort** is an efficient algorithm for sorting a small number of elements. We iterate through the list and if current element is smaller than the previous elements then we put that element at it's correct position.

![Insertion Sort]({{ site.url }}/assets/pythoninsertionsort1.png)

Algorithm:

```python
def insertion_sort(thelist):
    """
    Insertion sort algorithm
    """
    if len(thelist) == 0:
        print("Empty list!!")

    elif len(thelist) == 1:
        print("Only one element!!")

    else:
        for index in range(1, len(thelist)):
            key = thelist[index]
            position = index - 1
            while position >= 0 and thelist[position] >= key:
                thelist[position + 1] = thelist[position]
                position = position - 1
            thelist[position + 1] = key
 ```
 
 In the above picture, the black rectangles holds value ```thelist[index]``` which is compared (`thelist[position] >= key`) with values in shaded rectangles to its left. The image is from the book *Introduction to Algorithms*.
 
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

def insertion_sort(thelist):
    """
    Insertion sort algorithm
    """
    if len(thelist) == 0:
        print("Empty list!!")

    elif len(thelist) == 1:
        print("Only one element!!")

    else:
        for index in range(1, len(thelist)):
            key = thelist[index]
            position = index - 1
            while position >= 0 and thelist[position] >= key:
                thelist[position + 1] = thelist[position]
                position = position - 1
            thelist[position + 1] = key

if __name__ == '__main__':
    input_list = get_input()
    insertion_sort(input_list)
    print(*input_list, sep = ", ")
```

Output:

![Output]({{ site.url }}/assets/pythoninsertionsortout.png)

