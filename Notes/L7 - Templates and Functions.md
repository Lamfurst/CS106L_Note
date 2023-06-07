[toc]

# Implicit Interface & Concepts

***

> Implicit Interface 定义了一组函数和类型的要求，但与传统的 Interface 不同的是，它不需要类型明确声明它实现了该 Interface，而是由编译器根据该类型的成员函数和类型来确定该类型是否符合 Implicit Interface 的要求。——ChatGPT

**Question:** What are we assuming (Implicit Interface) here about the `typename InputIterator` and `typename DataType` here? 

![image-20230116214746362](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230116214746362.png)**Answer:** 

![image-20230205151948480](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230205151948480.png)

**Notice:** There will be a lot of errors if we input some variable with type that do not support all of above. For example, if we input string for InputIterator, string does not support `++`.

***

# C++20 Concepts: named requirements on the template arguments

```cpp
template <typename It, typename Type>
	requires Input_Iterator<It> && Iterator_of<It> && 
	Equlity_comparable<Value_type<It>>, Type>
```

**Notice:** A good thing about this is that program will not throw a huge error. Instead, it will just throw simple error from the `requires`.

***

# Functions and Lambda

***

## Predicate

**Def:** A function that takes in some number of arguments and returns a boolean.

**Example:**

```cpp
bool isEqualTo3(int val){
    return val == 3;
}
```

***

## Passing by Function Pointer

**With Predicate, we can lift the concept of the former question.** 

How many times does the element **[type] [val]** appears in  [a range of elements]

$\rightarrow$

How many times does the element satisfy **[predicate]** in  [a range of elements]

**Example:**

```cpp
template <typename InputIterator, typename UnaryPredicate>
int countOccurence(InputIterator begin, InputIterator end, UnaryPredicate predicate)
{
    int count = 0;
    for (auto iter = begin; iter != end; ++iter)
    {
        if(predicate(*iter)) ++count;
    }
    return count;
}

bool isEqualTo3(int val){
    return val == 3;
}

int main()
{
    vector<int> vec{1, 3, 5, 7, 9};
    countOccurence(vec.begin(), vec.end(), isEqualTo3);
    // return 1;
    return 0;
}
```

**Notice:** This called passing a function pointer.

***

## Problem with Function Pointer:

What if we want to pass in two values as variables for `predicate`?

**Example:**

```cpp
bool isEqualTo(int val, int val2){
    return val == val2;
}
// This won't compile. Instead, this throws an error "isEqualTo is not a unary function"
// Because the we define the predicate to be Unaray (with one input in template function)
```

**Solution: Functor/Function Object (Pre-C++11):**

```cpp
class GreaterThan
{
    public:
    	GreaterThan(int limit) : limit(limit){}
    	bool operator() (int val) {return val >= limit;}
    private:
    	int limit;
};

int main()
{
    int limit = 3;
    vector<int> grades{1,3,4,5,601,2};
    GreaterThan func(limit);
    cout << countOccurence(grades.begin(), grades.end(), func) << endl;
    return 0;
}
```

**Solution (C++11):**

**Lambda Function!**

```CPP
int main()
{
    int limit = 3;
    vector<int> grades{1,3,4,5,601,2};
    auto func = [limit](auto val) {return val >= limit;};
    // This is an object acting like function.
    countOccurence(grades.begin(), grades.end(), func);
}
```

***

# Lambda Function

## Anatomy of Lambda Function

```cpp
auto func = [capture-clause](parameters) -> return-value {
    // body
};

// return-value is not a must
// So this can also be as following
```

```cpp
auto func = [capture-clause](parameters) {
    // body
};
// C++ actually write a class with unknown name us Pre-C++11 solution under the hood
```

**Notice:** 

- `capture-clause` is passing the parameter into lambda function

- We can also pass parameters as reference into `capture-clause`

  Example:

  ```cpp
  set<string> teas{"black", "green", "oolong"};
  auto teaType = [&tea](auto type) {
      return tea.count(type);
  };
  ```

  If you have lot of parameters to pass in [], you can assign pass by value or pass by reference in the following way (Not a good idea though):
  
  ```cpp
  auto teaType = [&](auto type) {}; // All by reference
  auto teaType = [=](auto type) {}; // All by value
  auto teaType = [=, &param1](auto type) {}; // All by value except for param1
  auto teaType = [&, =param1](auto type) {}; // All by reference except for param1
  ```
  
  





 
