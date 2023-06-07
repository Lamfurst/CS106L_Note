[toc]

# Special Member Function

> Special member functions are member functions that are implicitly defined as member of classes under certain circumstances. There are six:
>
> | Member function                                              | typical form for class `C`: |
> | :----------------------------------------------------------- | :-------------------------- |
> | [Default constructor](https://cplusplus.com/doc/tutorial/classes2/#default_constructor) | `C::C();`                   |
> | [Destructor](https://cplusplus.com/doc/tutorial/classes2/#destructor) | `C::~C();`                  |
> | [Copy constructor](https://cplusplus.com/doc/tutorial/classes2/#copy_constructor) | `C::C (const C&);`          |
> | [Copy assignment](https://cplusplus.com/doc/tutorial/classes2/#copy_assignment) | `C& operator= (const C&);`  |
> | [Move constructor](https://cplusplus.com/doc/tutorial/classes2/#move) | `C::C (C&&);`               |
> | [Move assignment](https://cplusplus.com/doc/tutorial/classes2/#move) | `C& operator= (C&&);`       |

# Constructor and Initialization List

In C++, initialization lists are used to initialize member variables, especially reference member variables, in the constructor of a class. Here are three main reasons why initialization lists should be used:

## 1. Initializing Reference Member Variables

When a class has a reference member variable, it must be initialized using an initialization list. A reference must be bound to an object when it is created and cannot be changed later. By using an initialization list, we ensure that the reference is properly initialized with a valid object.

Example:

```cpp
class MyClass {
private:
    int& ref;

public:
    MyClass(int& value) : ref(value) {
        // Constructor body
    }
};
```

## 2. Initializing `const` Member Variables

When a class has a `const` member variable, it must be initialized using an initialization list, as `const` variables cannot be assigned a value after they have been initialized. 

Example:

```cpp
class MyClass {
private:
    const int value;

public:
    MyClass(int val) : value(val) {
        // Constructor body
    }
};
```

## 3. Efficient Initialization

Initialization lists can also be used to efficiently initialize member variables. By initializing member variables directly in the initialization list, you avoid the extra step of default initialization followed by assignment in the constructor body. This can lead to improved performance, especially for complex objects or types with expensive constructors.

Example:

```cpp
class MyComplexClass {
private:
    std::vector<int> data;

public:
    MyComplexClass() : data(1000, 0) {
        // Constructor body
    }
};
```

# Copy construction

Object is created as a copy of an existing object

# Copy assignment

Existing object replaced as a copy of another existing object

# Delete Operation

```cpp
// Example
// If you don't want copy ctor
class LoggedVector{
    public:
    LoggedVector(const LoggedVector& rhs) = delete;
}
```

# Copy Elision and return value optimization (RVO)

> Return Value Optimization (RVO) is an optimization technique employed by compilers to eliminate unnecessary copies of objects when returning them from a function. It allows the compiler to optimize the code and construct the return value directly in the memory location of the caller, avoiding extra copies.

**Example:**

```cpp
#include <iostream>

// Simple class with expensive copy constructor
class MyClass {
public:
    MyClass() {
        std::cout << "Default constructor called." << std::endl;
    }

    MyClass(const MyClass& other) {
        std::cout << "Copy constructor called." << std::endl;
    }
};

MyClass createObject() {
    MyClass obj;
    return obj;
}

int main() {
    MyClass newObj = createObject();
    return 0;
}
```

**Explanation:**

In the above example, we have a class `MyClass` with a default constructor and a copy constructor. The `createObject()` function creates an object of `MyClass` and returns it by value. In the `main()` function, the returned object is assigned to `newObj`.

Without Return Value Optimization, the code would require a copy of the object during the return statement, resulting in a call to the copy constructor. However, with Return Value Optimization enabled, the compiler optimizes the code and constructs the object `newObj` directly in the memory location where the return value is expected, eliminating the need for a copy.

When you run the program, if Return Value Optimization is applied, you will see that only the default constructor is called, indicating that the copy constructor is not invoked. This demonstrates the optimization achieved by avoiding unnecessary copies.