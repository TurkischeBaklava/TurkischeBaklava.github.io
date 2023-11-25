---
title: Placement new
date: 2023-11-25 16:26:29 +/-TTTT
---

**What?**

Normally, when an object is created dynamically or statically, an allocation function is invoked in such a way that it will both allocate memory for the object and initalize the object within the newly allocated memory. _[wiki]_

However we can seperate these two cruical processes by using placement new, thus separating memory placement from object construction. Placement syntax simply allows the programmer to supply additional arguments to allocation function.

The simplest use is to place an object at a particular location in memory. 
This is done by supplying the place as a pointer parameter to the placement new. 


<br/>

Placement new does not allocate memory, it only calls a constructor on existing memory. That existing memory could be on the stack or the heap — it does not matter. It is programmer's reponsibility to set aside the memory somewhere.

```c++
// Stack allocated
char buffer_stack [10 * sizeof(int)];
int* foo = new (buffer_stack) int[10];

// Heap allocated
char* memory = new char [10 * sizeof(int)];
void* place = memory;   // This step is just here to make code obvious
int* f = new (place) int[10];
```

It conceptually works similar to ``malloc()``. Size is calculated manually whereas compiler calculates the size for when the new operator is called.

<br/>

**Deallocation**

The deallocation is done using delete operation when allocation is done by new but there is no placement delete, but if it is needed one can write it with the help of destructor.
The reason that there is no built-in “placement delete” to match placement new is that there is no general way of assuring that it would be used correctly.


<br/>

**Where to use?**

Don’t use this “placement new” syntax unless you have to. Use it only when you really care that an object is placed at a particular location in memory. For example, when your hardware has a memory-mapped I/O timer device, and you want to place a Clock object at that memory location. _[Bjarne_Dtors]_

Or there may be cases when it is required to re-construct an object multiple times so, placement new function could be more efficient in this case.

Reserving a memory block like std::vector<>::reserve could be another example.

<br/><br/>

**Bjarne_Dtors** https://isocpp.org/wiki/faq/dtors#placement-new

**wiki** https://en.wikipedia.org/wiki/Placement_syntax#:~:text=%2D%3E~T()%3B-,Use%20cases,the%20object%20to%20be%20constructed.

https://cplusplus.com/forum/beginner/165813