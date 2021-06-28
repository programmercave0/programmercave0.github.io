---
layout: post
title: "Kefa and First Steps - CodeForces | C++ Implementation"
subtitle: "Kefa decided to make some money doing business on the Internet for exactly *n* days. He knows that on the *i*-th day (1≤*i*≤n) he makes *a<sub>i</sub>* money. Kefa loves progress, that's why he wants to know the length of the maximum non-decreasing subsegment in sequence *a<sub>i</sub>*. Let us remind you that the subsegment of the sequence is its continuous fragment. A subsegment of numbers is called non-decreasing if all numbers in it follow in the non-decreasing order."
author: "Programmercave"
header-img: ""
tags:  [Cpp, Competitive-Programming, CodeForces, Dynamic-Programming]
date: 2021-06-28
---

<h1>Problem:</h1>

Kefa decided to make some money doing business on the Internet for exactly *n* days. He knows that on the *i*-th day (1≤*i*≤n) he makes *a<sub>i</sub>* money. Kefa loves progress, that's why he wants to know the length of the maximum non-decreasing subsegment in sequence *a<sub>i</sub>*. Let us remind you that the subsegment of the sequence is its continuous fragment. A subsegment of numbers is called non-decreasing if all numbers in it follow in the non-decreasing order.

<h3>Input</h3>
The first line contains integer *n*.

The second line contains *n* integers *a<sub>1</sub>*, *a<sub>2</sub>*, ..., *a<sub>n</sub>*

<h3>Output</h3>

Print a single integer — the length of the maximum non-decreasing subsegment of sequence *a*.

Read full problem here : [Kefa and First Steps](https://codeforces.com/problemset/problem/580/A)

<h1>Solution:</h1>

Here we have to find the length of the maximum increasing subsegment in sequence. We will do the counting while we enter values first time. 

Let `earnings` is an integer vector of size `n` which stores amount earned on each day. 

Before processing we need three integer variables. `current_earning` stores highest amount in an increasing subsegment. It's value is compared with `earnings[i]` everytime new value is entered.

`final_count` stores length of longest increasing subsegment. `local_count` stores length of current subsegment in process.

```
int current_earning; //stores highest earning of non-decreasing subsegment
int final_count = 0; //stores length of longest non-decreasing subsegment
int local_count = 1; //stores length of current subsegment in process
```

We will run a loop from `0` to `n-1` to enter values in `earnings` and `earnings[i]` is the current value entered.

At `i=0`, the value of `current_earning` is `earnings[i]`.

```
if (i == 0)
{
	current_earning = earnings[i];
}
```

Else, if the value of `earnings[i]` is greater than or equal to `current_earning` then we will increase value of `local_count` by 1. Because currently we have an increasing subsegment. And the value of `current_earning` will become the value of `earnings[i]`, to help us to compare with next value entered in `earnings`.

```
if (i == 0)
{
	...
}
else
{
	if (earnings[i] >= current_earning)
	{
		local_count++;
		current_earning = earnings[i];
	}
	else
	{
		...
	}
}
```

If value of `earnings[i]` is less than `current_earning` means the increasing subsegment ends at `i-1`. So we compare values of `local_count` and `final_count`. If `local_count` is greater than `final_count` means we found new increasing subsegment whose length is greater than the previous one.

```
if (i == 0)
{
	...
}
else
{
	if (earnings[i] >= current_earning)
	{
		...
	}
	else
	{
		if (local_count > final_count)
		{
			final_count = local_count;
		}
		local_count = 1;
		current_earning = earnings[i];
	}
}
```

<h3>C++ Implementation</h3>

```cpp
#include <iostream>
#include <vector>

int main()
{
	int n;
	std::cin >> n;

	std::vector<int> earnings(n);

	int current_earning; //stores highest earning of non-decreasing subsegment
	int final_count = 0; //stores length of longest non-decreasing subsegment
	int local_count = 1; //stores length of current subsegment in process

	for (int i = 0; i < n; ++i)
	{
		std::cin >> earnings[i];

		if (i == 0)
		{
			current_earning = earnings[i]
			//local_count++;
		}
		else
		{
			if (earnings[i] >= current_earning)
			{
				local_count++;
				current_earning = earnings[i];
			}
			else
			{
				if (local_count > final_count)
				{
					final_count = local_count;
				}
				local_count = 1;
				current_earning = earnings[i];
			}		
		}
	}
	if (local_count > final_count)
	{
		final_count = local_count;
	}
	std::cout << final_count << "\n";
}
```

View this on [Github](https://github.com/programmercave0/Competitive-Programming/edit/master/Codeforces/Kefa_and_first_steps.cpp)

If you have another solution then please share it in the comments below.