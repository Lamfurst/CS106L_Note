[toc]

# lvalues vs. rvalues (simplified)

## l-value

An l-value is an expression that has a name (identity).

- Can find address using address-of operator (&var)
- Can appear on both side of assignment

## r-value

An r-value is an expression that does not have a name (identity). 

- Temporary values 
- Cannot find address using address-of operator (&var)
- Can only appear on the right side of the assignment

**Example:**

<img src="C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230604165717077.png" alt="image-20230604165717077" style="zoom:50%;" />

**Notice: *ptr is a l-value**

## l-value reference vs. r-value reference

<img src="C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230604170322073.png" alt="image-20230604170322073" style="zoom:50%;" />

## Why r-values are key to move semantics

- An object that is an l-value is NOT disposable, so you can copy from, but definitely cannot move from
- An object that is an r-value is disposable, so you can either copy o move from.

# `move constructor` and `move assignment`

##  Move Constructor

The move constructor is a special constructor in C++ that allows the efficient transfer of resources from one object to another. It is used when the source object is no longer needed and its resources can be moved to a new object without the need for copying or duplicating.

### Implementation

The move constructor for the `MyVector` class can be defined as follows:

```cpp
MyVector<T>(MyVector<T>&& other) :
    elems(std::move(other.elems)),
    logicalSize(std::move(other.logicalSize)),
    allocatedSize(std::move(other.allocatedSize)) {
    other.elems = nullptr;
}
```

In this implementation:

- `MyVector<T>&& other` indicates an r-value reference to another `MyVector` object, allowing the efficient transfer of resources.
- `std::move` unconditionally cast to an r-value. Actually the `=` does the job of `move`
- The `elems`, `logicalSize`, and `allocatedSize` member variables of the `other` object are moved to the corresponding member variables of the new object.
- After transferring the resources, `other.elems` is set to `nullptr` to indicate that the resources have been moved.

## Move Assignment Operator

The move assignment operator is another special member function in C++ that enables efficient transfer of resources from one object to another. It is used when an existing object already has resources allocated, and another object needs to take ownership of those resources.

### Implementation

The move assignment operator for the `MyVector` class can be implemented as follows:

```
cppCopy codeMyVector<T>& operator=(MyVector<T>&& other) {
    if (this != &other) {
        delete[] elems;  // Deallocate existing resources
        elems = std::move(other.elems);
        logicalSize = std::move(other.logicalSize);
        allocatedSize = std::move(other.allocatedSize);
        other.elems = nullptr;
    }
    return *this;
}
```

In this implementation:

- `MyVector<T>& operator=(MyVector<T>&& other)` defines the move assignment operator that takes an rvalue reference to another `MyVector` object.
- `if (this != &other)` checks if the source object is different from the current object to avoid self-assignment.
- `delete[] elems` deallocates any existing resources in the current object.
- `elems = std::move(other.elems)`, `logicalSize = std::move(other.logicalSize)`, and `allocatedSize = std::move(other.allocatedSize)` move the resources from the `other` object to the current object.
- `other.elems = nullptr` sets the `other` object's resources to `nullptr` after transferring ownership.

# Swap

```cpp
template <typename T>
void swap(T& a, T&b)
{
    T temp = std::move(a);
    a = std:move(b);
    b = std::move(temp);
}
```







