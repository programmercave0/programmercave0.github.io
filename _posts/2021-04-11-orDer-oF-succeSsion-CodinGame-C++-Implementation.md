---
layout: post
title: "orDer oF succeSsion - CodinGame | C++ Implementation"
subtitle: "You have to output the order of succession to the British throne of a list of given people. The order is simple:
From a descendant A, the next in the order is A’s first child B.
Then, the next one is B’s first child C if any and so on.
If C has no child, then the next one is B’s second child D.
Then D’s children if any. Then B’s third child E… then A’s second child F"
author: "Programmercave"
header-img: "/assets/2021-04-11-orDer-oF-succeSsion-CodinGame/order_of_succession4.jpg"
tags:  [Cpp, Competitive-Programming, CodinGame]
date: 2021-04-11
---

The problem is from [CodinGame](https://www.codingame.com/home) with difficulty level Medium.

<h1>Problem:</h1>

You have to output the order of succession to the British throne of a list of given people.
The order is simple:
From a descendant A, the next in the order is A’s first child B.
Then, the next one is B’s first child C if any and so on.
If C has no child, then the next one is B’s second child D.
Then D’s children if any. Then B’s third child E… then A’s second child F…

Let’s draw it with a tree:

![orDer oF succeSsion]({{ site.url }}/assets/2021-04-11-orDer-oF-succeSsion-CodinGame/order_of_succession1.jpg){:class="img-responsive"}

You see the order of succession: begin on the left of the tree, walk to the next level whenever possible otherwise continue to the right. Repeat until the whole tree is covered.
Thus, the order is A-B-C-D-E-F.

In fact, in siblings of the same person, the male descendants are ordered before the female descendants. For example, if the order of birth of the children (M for male, F for female) is Fa Ma Me Fe then the order of succession in these siblings is Ma Me Fa Fe.

**Ordering rules**
(a) in order of generation
(b) in order of gender
(c) in order of age (year of birth)

**Outputting rules**
(a) exclude dead people (but include siblings of dead people)
(b) exclude people who are catholic (but include siblings of catholic people)

**Constraints**
Exactly one people does not have a parent (the parent’s name is replaced by the hyphen ```-```).

Read full problem here : [orDer oF succeSsion](https://www.codingame.com/ide/puzzle/order-of-succession)

<h1>Solution:</h1>

Each person has name, parent name, year of birth and death etc. We will store these details in a struct Details.

```cpp
struct Details
{
    std::string name;
    std::string parent_name;
    int birth; // year of birth
    std::string death;
    std::string religion;
    std::string gender;
    int index;
    bool has_child = false;
    bool processed = false;

    Details * parent;
    Details * sibling;
    Details * first_child;

    Details(){}

    Details(std::string name_, std::string parent_name_, int birth_, std::string death_, std::string religion_, std::string gender_, int index_):
        name(name_), parent_name(parent_name_), birth(birth_), death(death_), religion(religion_), 
        gender(gender_), index(index_), parent(nullptr), sibling(nullptr), first_child(nullptr) {}
};
```

In ```main``` function, we enter data in a vector of ```Details```.  
```cpp
std::vector<Details> family_details;
```

The person whose parent doesn’t exist in ```family_details``` is the first ruler. We will store the index of that person in an integer variable ```first_ruler_index```.

```cpp
int first_ruler_idx;

for (int i = 0; i < n; i++) 
{
    std::string name;
    std::string parent;
    int birth;
    std::string death;
    std::string religion;
    std::string gender;
    std::cin >> name >> parent >> birth >> death >> religion >> gender; std::cin.ignore();

    if (parent == "-")
    {
        first_ruler_idx = i;
    }

    family_details.push_back( Details(name, parent, birth, death, religion, gender, i));
        
}
```

<h3> Function order_of_succession: </h3>

In this function, we have passed vector of ```Details``` and an integer variable ```first_ruler_idx``` as parameters. This function returns vector of strings in the order of succession.

First we declare a vector of strings ```rulers``` and an integer variable ```curr_ruler_idx```.

Then it calls ```map_parent_children``` function.

Function ```map_parent_children``` will map the ruler at ```curr_ruler_idx``` with his/her children.

Now we will start ordering rulers. 

A person is eligible to rule if he/she is not dead, his/her religion is not Catholic. While processing we have to make sure the same person is not processed twice, so boolen variable ```processed``` for that person is set to true once he/she is processed.

All this processing is done in a ```while``` loop, whose condition is always ```true```.

```cpp
while (true)
{
    if (family[curr_ruler_idx].death == "-" && 
        family[curr_ruler_idx].religion != "Catholic" && !family[curr_ruler_idx].processed)
    {
        rulers.push_back(family[curr_ruler_idx].name);
    }
    family[curr_ruler_idx].processed = true;

    ...
    ...
    ...
}
```

Once the ruler at ```curr_ruler_idx``` is added to the vector ```ruler```. We have to check if he/she has a child or not. If he/she has child then the value of ```curr_ruler_idx``` is updated to the index of the ```first_child```. 

The siblings are already ordered according to their age and gender in ```map_parent_children``` function.

```cpp
while (true)
{
    ...

    if (family[curr_ruler_idx].has_child)
    {
        curr_ruler_idx = family[curr_ruler_idx].first_child->index;
    }
    else
    {
        ...
    }
    map_parent_children(family, curr_ruler_idx); 
}
```

If current ruler does not have a child then we will check for his/her sibling. If he/she has sibling then ```curr_ruler_idx``` is updated to the index of next sibling.

```cpp
while (true)
{
    ...

    if (family[curr_ruler_idx].has_child)
    {
        ...
    }
    else
    {
        if (family[curr_ruler_idx].sibling != nullptr)
        {
            curr_ruler_idx = family[curr_ruler_idx].sibling->index;
        }
        else
        {
            ...
        }
    }
    map_parent_children(family, curr_ruler_idx); 
}
```

If current ruler does not have sibling, then sibling to his/her parent will be the next ruler. If his/her parent does not have sibling then we check for grandparent and so on.

![orDer oF succeSsion]({{ site.url }}/assets/2021-04-11-orDer-oF-succeSsion-CodinGame/order_of_succession2.jpg){:class="img-responsive"}

Before processing we have to make sure that the current ruler is not equal to first ruler. If he is equal to the first ruler then we have processed everyone in the family and we terminate the while loop.

```cpp
while (true)
{
    ...

    if (family[curr_ruler_idx].has_child)
    {
        ...
    }
    else
    {
        if (family[curr_ruler_idx].sibling != nullptr)
        {
            ...
        }
        else
        {
            while (curr_ruler_idx != first_ruler_idx &&
                 family[curr_ruler_idx].parent->sibling == nullptr)
            {
                curr_ruler_idx = family[curr_ruler_idx].parent->index;   
            }

            if (curr_ruler_idx == first_ruler_idx)
            {
                break;
            }

            if (family[curr_ruler_idx].parent->sibling != nullptr)
            {
                curr_ruler_idx = family[curr_ruler_idx].parent->sibling->index;
            }
        }
    }
    map_parent_children(family, curr_ruler_idx); 
}
```

In while loop we again call ```map_parent_children``` function because ```curr_ruler_idx``` is changed.

<h3> Function map_parent_children : </h3>

In this function, we have passed a vector of ```Details``` ```family``` and an integer variable ```curr_ruler_idx``` as parameter.

This function will map the ruler at ```curr_ruler_idx``` with his/her children.

![orDer oF succeSsion]({{ site.url }}/assets/2021-04-11-orDer-oF-succeSsion-CodinGame/order_of_succession3.jpg){:class="img-responsive"}

First we declare a vector of integers ```next_gen_m_idx``` which will store the indices of male children of the ruler at ```curr_ruler_idx```.

And vector of integers ```next_gen_f_idx``` will store the indices of female children of the ruler at ```curr_ruler_idx```.

```cpp
std::vector<int> next_gen_m_idx; // stores index of male members of next generation
std::vector<int> next_gen_f_idx; // stores index of female members of next generation
```

We will iterate through the vector ```family``` and if the name of the parent of the person at the current index is equal to the name of the ruler at ```curr_ruler_idx``` then the current ruler is the parent of the person in process. Then we will enter the index of the current person in vector ```next_gen_m_idx``` or ```next_gen_f_idx``` according to its gender.

```cpp
for (int i = 0; i < family.size(); ++i)
{
    if (family[i].parent_name == family[curr_ruler_idx].name)
    {
        family[curr_ruler_idx].has_child = true;
        family[i].parent = &family[curr_ruler_idx]; // mapping parent
        if (family[i].gender == "M")
        {
            next_gen_m_idx.push_back(i);
        }
        else
        {
            next_gen_f_idx.push_back(i);
        }
    }  
}
```

In siblings, the eldest sibling is the next ruler. So we will sort both vectors  ```next_gen_m_idx``` and  ```next_gen_f_idx``` according to there age. And then we will call function ```map_siblings```.

```cpp
//sort both vectors according to age
if (family[curr_ruler_idx].has_child)
{
    static const auto by_age = [family](const int i, const int j)
    {
        return family[i].birth < family[j].birth; // year of birth is smaller means age is bigger
    };   

    std::sort(next_gen_m_idx.begin(), next_gen_m_idx.begin(), by_age);
    std::sort(next_gen_f_idx.begin(), next_gen_f_idx.begin(), by_age);

    map_siblings(next_gen_m_idx, next_gen_f_idx, family, curr_ruler_idx);
}
```

<h3> Function map_siblings : </h3>

In this function two integer vectors ```male_idx``` and ```female_idx```, one vector of Details ```family``` and an integer variable ```curr_ruler_idx``` are passed as parameters.

The vectors ```male_idx``` and ```female_idx``` are already sorted according to ages. We need to form links between siblings.

![orDer oF succeSsion]({{ site.url }}/assets/2021-04-11-orDer-oF-succeSsion-CodinGame/order_of_succession4.jpg){:class="img-responsive"}

First we will check whether the vector ```male_idx``` is empty of not. If it is not empty then the ```first_child``` of current ruler is set to the child at the index ```0``` of vector ```male_idx```.

Then if the size of ```male_idx``` is 1 and vector ```female_idx``` is not empty then the sibling of child at index ```0``` of ```male_idx``` is child at the index ```0``` of vector ```female_idx```.

Otherwise we will iterate through vector ```male_idx``` and link siblings

```cpp
if (!male_idx.empty())
{
    family[curr_ruler_idx].first_child = &family[male_idx[0]];
    if (male_idx.size() == 1)
    {
        if (!female_idx.empty())
        {
            family[male_idx[0]].sibling = &family[female_idx[0]];
        }
    }
    else
    {
        for (int i = 0; i < male_idx.size()-1; ++i)
        {
            family[male_idx[i]].sibling = &family[male_idx[i+1]];
        }

    }
}
else
{
    ...
}
```

If vector ```male_idx``` is empty then first child of current ruler is the child at index ```0``` of vector ```female_idx```.

```cpp
if (!male_idx.empty())
{
    ...
}
else
{
    family[curr_ruler_idx].first_child = &family[female_idx[0]];
}
```

Now we will link last male child to the first female child, if number of male children is more than 1 and there is atleast one female child.

```cpp
if (!female_idx.empty())
{
    if (male_idx.size() > 1)
    {
        family[male_idx.back()].sibling = &family[female_idx[0]];
    }

    if (female_idx.size() > 1)
    {
        for (int i = 0; i < female_idx.size()-1; ++i)
        {
            family[female_idx[i]].sibling = &family[female_idx[i+1]];
        }
    }
}
```

{% include ads.html %}<br/>

<h3>C++ Implementation</h3>

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

struct Details
{
    std::string name;
    std::string parent_name;
    int birth; // year of birth
    std::string death;
    std::string religion;
    std::string gender;
    int index;
    bool has_child = false;
    bool processed = false;

    Details * parent;
    Details * sibling;
    Details * first_child;

    Details(){}

    Details(std::string name_, std::string parent_name_, int birth_, std::string death_, std::string religion_, std::string gender_, int index_):
        name(name_), parent_name(parent_name_), birth(birth_), death(death_), religion(religion_), 
        gender(gender_), index(index_), parent(nullptr), sibling(nullptr), first_child(nullptr) {}
};

void map_siblings(std::vector<int> & male_idx, std::vector<int> & female_idx, std::vector<Details>& family, int curr_ruler_idx)
{

    if (!male_idx.empty())
    {
        family[curr_ruler_idx].first_child = &family[male_idx[0]];
        if (male_idx.size() == 1)
        {
            if (!female_idx.empty())
            {
                family[male_idx[0]].sibling = &family[female_idx[0]];
            }
        }
        else
        {
            for (int i = 0; i < male_idx.size()-1; ++i)
            {
                family[male_idx[i]].sibling = &family[male_idx[i+1]];
            }

        }
    }
    else
    {
        family[curr_ruler_idx].first_child = &family[female_idx[0]];
    }

    if (!female_idx.empty())
    {
        if (male_idx.size() > 1)
        {
            family[male_idx.back()].sibling = &family[female_idx[0]];
        }

        if (female_idx.size() > 1)
        {
            for (int i = 0; i < female_idx.size()-1; ++i)
            {
                family[female_idx[i]].sibling = &family[female_idx[i+1]];
            }
        }
    }
}

void map_parent_children(std::vector<Details>& family, int curr_ruler_idx)
{
    std::vector<int> next_gen_m_idx; // stores index of male members of next generation
    std::vector<int> next_gen_f_idx; // stores index of female members of next generation
    
    for (int i = 0; i < family.size(); ++i)
    {
        if (family[i].parent_name == family[curr_ruler_idx].name)
        {
            family[curr_ruler_idx].has_child = true;
            family[i].parent = &family[curr_ruler_idx]; // mapping parent
            if (family[i].gender == "M")
            {
                next_gen_m_idx.push_back(i);
            }
            else
            {
                next_gen_f_idx.push_back(i);
            }
        }  
    }
    //sort both vectors according to age
    if (family[curr_ruler_idx].has_child)
    {
        static const auto by_age = [family](const int i, const int j)
        {
            return family[i].birth < family[j].birth; // year of birth is smaller means age is bigger
        };   

        std::sort(next_gen_m_idx.begin(), next_gen_m_idx.begin(), by_age);
        std::sort(next_gen_f_idx.begin(), next_gen_f_idx.begin(), by_age);

        map_siblings(next_gen_m_idx, next_gen_f_idx, family, curr_ruler_idx);
    }
}


std::vector<std::string> order_of_succession(std::vector<Details>& family, int first_ruler_idx)
{
    std::vector<std::string> rulers;
    int curr_ruler_idx = first_ruler_idx;

    map_parent_children(family, first_ruler_idx);

    while (true)
    {
        if (family[curr_ruler_idx].death == "-" && 
            family[curr_ruler_idx].religion != "Catholic" && !family[curr_ruler_idx].processed)
        {
            rulers.push_back(family[curr_ruler_idx].name);
        }
        family[curr_ruler_idx].processed = true;

        if (family[curr_ruler_idx].has_child)
        {
            curr_ruler_idx = family[curr_ruler_idx].first_child->index;
        }
        else
        {
            if (family[curr_ruler_idx].sibling != nullptr)
            {
                curr_ruler_idx = family[curr_ruler_idx].sibling->index;
            }
            else
            {
                while (curr_ruler_idx != first_ruler_idx &&
                    family[curr_ruler_idx].parent->sibling == nullptr)
                {
                    curr_ruler_idx = family[curr_ruler_idx].parent->index;   
                }

                if (curr_ruler_idx == first_ruler_idx)
                {
                    break;
                }

                if (family[curr_ruler_idx].parent->sibling != nullptr)
                {
                    curr_ruler_idx = family[curr_ruler_idx].parent->sibling->index;
                }
            }
        }
        map_parent_children(family, curr_ruler_idx); 
    }
   
    return rulers;
}

int main()
{
    int n;
    std::cin >> n; std::cin.ignore();

    std::vector<Details> family_details;
    int first_ruler_idx;

    for (int i = 0; i < n; i++) 
    {
        std::string name;
        std::string parent;
        int birth;
        std::string death;
        std::string religion;
        std::string gender;
        std::cin >> name >> parent >> birth >> death >> religion >> gender; std::cin.ignore();

        if (parent == "-")
        {
            first_ruler_idx = i;
        }

        family_details.push_back( Details(name, parent, birth, death, religion, gender, i));
        
    }

    std::vector<std::string> ruler_order = order_of_succession(family_details, first_ruler_idx);
    for (int i = 0; i < ruler_order.size(); ++i)
    {
        std::cout << ruler_order[i] << "\n";
    }
}
```

Note : One test case is not passed

View this on [Github](https://github.com/programmercave0/CodinGame-Solutions/blob/master/Order_of_Succession.cpp)

If you have another solution then please share it in the comments below.

<h3>Other Competitive Programming Problems and Solutions</h3>
[Stock Exchange Losses - CodinGame ]({{ site.url }}/blog/2021/03/17/Stock-Exchange-Losses-CodinGame-C++-Implementation)<br/>
[Dungeons and Maps - CodinGame]({{ site.url }}/blog/2021/03/01/Dungeons-and-Maps-CodinGame-C++-Implementation)<br/>

