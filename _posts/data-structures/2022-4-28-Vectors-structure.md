---
layout: post
title: Vectors definitions
published: true
---

Let's start this series on data structures with vectors. Or list? Or arrays?

The terminology and definitions may vary so let's start by talking about this. 
The definition depends from where it comes form and what language we are talking about. 

For example C++ defines vector with in `<vector>` and it has the following properties:
- variable size
- non indexed
- "unefficient" access
- occupies more memory than necessary


The array is pretty much the complete opposite on these points.

The implementation we will see of vectors actually take from the two. 
It has the following properties:
- variable size
- indexed
- fast access
- occupies more memory than necessry

We could use the following structure:  
```c
struct vector
{
    size_t size;
    void **data;
};
```
And use `realloc` or `reallocarray` to increase the size of `data` every time we want to insert or remove an element.  
But this method causes optimization issues. Let's review some.

### Problem 1:
For example, let's imagine you want to append (insert at the end) elements successively. 
But realloc has a complexity of O(size). So the insert function aswell.

### Problem 2:
Now, let's imagine you have a few elements in the list. 
Then append an element and pop it multiple times in a row. 
And the pop function also uses `realloc` to decrease the size. 
In this situation you will realloc everytime you do an action on the list, with a complexity of O(n) for each.

Let's see another declaration of the structure that may help us:
```c
struct vector
{
    size_t capacity;
    size_t size;
    void **data;
};
```

Here the size is how much element the vector currently has, the capacity is how much element the vector can contain at most.  
We don't need to use `realloc` evrerytime anymore.
If we insert and the capacity is equal to the size we multiply the capacity and `realloc` data to a bigger size. 
And similarly for the inverse case, if the size is too inferior to the capacity, you `realloc` data to a smaller size.

Thanks to this the insertion has a linearized complexity of O(1), same for pop.

We still have a few question left about this:
1. By what factor do we multiply the size? 
2. By what factor do we divide the size?
3. What does "the size is too inferior to the capacity" mean?

Well it all depends on the situation and the application but the factor is often 2, and too small is: `size==(capacity/2)*0.75`.  

The array reduction when the size is too small is not mandatory, it just makes it more memory efficient. 
The array expantion on the other side is rather mandatory if you want to make it mandatory if you want a real dynamic size.


This will be it for this article. Next time we will probably talk about the "methods" on this structure.