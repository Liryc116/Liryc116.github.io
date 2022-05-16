---
layout: post
title: Vectors methods
published: true
---

Today's topic is the methods of vectors. 
We will start with the constructor and destructors.
(note that the terms methods, constructor and destructor aren't technically correct since C doesn't have Object Oriented Programming but it's close enough).

Here is the constructor:

```C
struct vector *vector_new()
{
    struct vector* vect = calloc(1, sizeof(struct vector));
    if (vect != NULL)
        vect->data = calloc(1, sizeof(void*));

    if (vect==NULL || vect->data==NULL)
        errx(1, "Not enough memory!");

    vect->capacity = 1;
    return vect;
}
```

We initiate the vectora as empty with a capacity of 1.
Using `calloc` avoid setting size as 0.  

The destructor is as follow:

```c
void vector_free(struct vector *v, void(*free_function)(void*))
{
    for(size_t i = 0; i<v->size; i++)
        free_function(v->data[i]);
    free(v->data);
    free(v);
}
```

We free all the data we allocate to avoid any memory leaks. The vector shall contain only one type else the `free_function` may not work correctly.

These two are pretty straight forward, just allocate and initialize the fileds in the constructor and free in the destructor.

There are few methods on vectors:
- push (or append), insert at the end
- pop, delete at the end
- get, returns the element stored at an index
- insert, insert at the index in parameter
- remove, deletes at the index in parameter

Push and pop are very similar to insert and remove, we just need to shift the elements in the latters.  

```c
void vector_push(struct vector *v, void *x, size_t elm_size)
{
    if (v->capacity == v->size)
        double_capacity(v);
    v->data[v->size] = malloc(elm_size);
    memcpy(v->data[v->size], x, elm_size);
    v->size+=1;
}
```  
Let's analyze the push: if the capacity and size are equal we double the capacity. 
Then we allocate the necessary space for the new element. 
We copy the data from the element we insert to the space we allocated.
Finally, we increment the size.

```c
void* vector_pop(struct vector *v)
{
    if(v->size>0)
    {
        v->size -= 1;
        void* x = v->data[v->size];
        v->data[v->size] = NULL;

        return x;
    }

    return NULL;
}
```
This is the pop. 
If the size is 0 we just send `NULL`. 
Else we decrement the size, memorize the pointer and change the stored pointer (this last part is not mandatory).
Since we allocated memory in the push and we aren't asking for a free, we just return the poped element.
The caller MUST free the returned element.
As detailled [here](https://liryc116.github.io/Vectors-structure/) reducing the allocated size to `v->data` is not mandatory. 