---
layout: post
title: Data Structure
published: true
---
I would like to dedicate a series of post to data structures. 

We will discuss their properties, their assiocated methods, their use cases. We will see my personal generic implementation of the data structures in C and understand it.

You can take a peek at the repository with all my code [here](https://github.com/liryc116/algoc)

Some details may come later as this series progress.

Here's a non-exhaustive list of the structure I want to talk about:

 - Vectors
 - Linked Lists
 - Doubly Linked Lists
 - Queues
 - Stack
 - Heap
 - Binary Trees
 - Binary Search Trees (BST)
 - AVL (Balanced BST)
 - Bicolor Tree / Red Black tree
 - B Trees
 - General Trees
 - Graphs
 - Hash Tables

## Let's talk a bit about the basics

How do you make a generic data structure in C?  
You simply use `void *` type.
 
`void *` is a pointer but what if we want to pass information that isn't malloced?  
Then just give the address of the element you want to insert with `&` in front.  
This is nice but once we escapre the scope of definition of this variable it will be lost.  
In this case we need to malloc every element inside the data structure "methods". And then we use `memcpy` to copy the input information to the memory we allocated. 
 
But `memcpy` needs a size of the data to copy. 
The initial guess would be to use `sizeof(void *)`.
But this doesn't give the size of the effective element we want to copy.

For example let's try the following code:

```c
void *a = malloc(sizeof(int));
int *b = malloc(sizeof(int));
printf("a = %lu\nb = %lu\n", sizeof(*a), sizeof(*b));  
```

The result is:
>a=1  
b=4

So if we wanted to insert an int we would just allocate it's first byte.  
The solution is to pass the data size as an argument when adding elements to the structure.

That's it for this article. We'll meet again soon for more data structures.