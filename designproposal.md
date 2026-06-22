# Custom Template Library (CTL)

## Technical Design Proposal

## 1. Summary

Custom Template Library (CTL) is a C++ STL-inspired generic container
framework designed to provide reusable and type-safe data structures.
The library implements custom DynamicArray, LinkedList, and HashMap
containers with manual memory management and optimized performance
strategies.

## 2. Objectives

-   Build reusable containers using C++ templates
-   Implement efficient memory management
-   Maintain optimized time complexity
-   Support extensible container design

### Template Design

Templates allow containers to support multiple data types using a single
implementation while maintaining compile-time type safety.

------------------------------------------------------------------------

# System Architecture Overview

The Custom Collections Library (CTL) is designed using a modular layered
architecture that separates public interfaces, container
implementations, and memory management responsibilities.

The structure focuses on reusable generic containers, controlled
resource handling, and efficient data storage mechanisms.

------------------------------------------------------------------------

# 3. API Specification

## DynamicArray`<T>`

  ---------------------------------------------------------------------------
  Category                Methods                     Complexity
  ----------------------- --------------------------- -----------------------
  Constructors            DynamicArray(),             O(1), Fill/Copy: O(n)
                          DynamicArray(size,value),   
                          Copy Constructor,           
                          operator=(), Destructor     

  Insert                  push_back(), insert()       O(1) amortized, O(n)

  Delete                  pop_back(), erase(),        O(1), O(n)
                          clear()                     

  Access                  operator[](), at(),         O(1)
                          front(), back()             

  Capacity                size(), capacity(), empty() O(1)
  ---------------------------------------------------------------------------

------------------------------------------------------------------------

## LinkedList`<T>`{=html}

  -----------------------------------------------------------------------
  Category                Methods                 Complexity
  ----------------------- ----------------------- -----------------------
  Constructors            LinkedList(), Copy      O(1), Copy: O(n)
                          Constructor,            
                          operator=(), Destructor 

  Insert                  push_front(),           O(1), O(1), O(n)
                          push_back(), insert()   

  Delete                  pop_front(),            O(1), O(n)
                          pop_back(), erase(),    
                          clear()                 

  Access                  front(), back()         O(1)

  Search                  search()                O(n)

  Capacity                size(), empty()         O(1)
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## HashMap\<Key, Value, Hash\>

  -----------------------------------------------------------------------
  Category                Methods                 Complexity
  ----------------------- ----------------------- -----------------------
  Constructors            HashMap(), Copy         O(1), Copy: O(n)
                          Constructor,            
                          operator=(), Destructor 

  Insert                  insert(key,value)       O(1) average, O(n)
                                                  worst

  Delete                  erase(key)              O(1) average, O(n)
                                                  worst

  Search                  find(), contains()      O(1) average, O(n)
                                                  worst

  Access                  operator\[\]            O(1) average

  Capacity                size(), empty(),        O(1)
                          loadFactor()            
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 4. Design Decisions

## DynamicArray

-   Uses contiguous heap memory for fast random access.
-   Initial capacity of 4 reduces unnecessary initial memory allocation.
-   Capacity increases by 1.5x during resizing.
-   This balances memory usage while maintaining amortized O(1)
    insertion.
-   Supports STL-like initialization.
-   Uses a 1.0 load factor because allocated memory can be completely
    utilized without affecting access complexity.

------------------------------------------------------------------------

## LinkedList

-   Implemented as a singly linked list with head and tail pointers.
-   Tail pointer improves insertion at the end from O(n) to O(1).
-   Doubly linked list avoided to reduce extra pointer memory overhead.

------------------------------------------------------------------------

## HashMap\<Key, Value, Hash\>

-   Uses template-based hash policy.
-   Supports custom hash functions for user-defined keys.

### Collision Handling

Decision: Separate Chaining

Reasons:

-   Handles high collision scenarios efficiently.
-   Avoids clustering problems.
-   Provides stable node-based storage.

### Load Factor

Maximum load factor = 0.75

This balances memory usage and collision reduction. When the load factor
exceeds 0.75, bucket size increases and existing keys are rehashed.

Initial bucket capacity = 8

This provides enough bucket positions for small datasets while avoiding
excessive memory allocation.

------------------------------------------------------------------------

# Memory Management

Implements Rule of Three:

-   Destructor
-   Copy Constructor
-   Copy Assignment Operator

This ensures: - Deep copying - Safe ownership handling - Prevention of
memory leaks

------------------------------------------------------------------------

# Internal Representation

## `DynamicArray<T>`

Uses contiguous heap memory where elements are stored sequentially.
Maintains:

-   Data pointer
-   Current size
-   Capacity

for efficient resizing.

## `LinkedList<T>`

Uses node-based heap allocation where every node contains:

-   Data
-   Next pointer

The container maintains head and tail pointers.

## HashMap\<Key, Value, Hash\>

Uses a bucket array where every bucket points to a chain of key-value
nodes. Collisions are resolved using separate chaining.

------------------------------------------------------------------------

# Conclusion

CTL provides an STL-inspired generic container library built using
templates, hashing abstraction, and efficient memory management.

The design ensures scalability, type safety, optimized performance, and
reusable container architecture.
