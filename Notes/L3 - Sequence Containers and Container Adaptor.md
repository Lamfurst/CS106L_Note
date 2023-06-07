[toc]

# Overview of STL

![image-20230106034847885](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230106034847885.png)

***

# Sequence Containers

**Includes:**

- `std::vector<T>`
- `std::deque<T>`
- `std::list<T>`
- `std::array<T>`
- `std::forward_list<T>`

***

# `std::vector<T>`

![image-20230106064221732](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230106064221732.png)

## Difference between `v.at(i)` and `v[i]`

`.at(i)` will check whether the index is out of the boundary or not. `[i]` will not do that.

**Example:**

```cpp
#include <iostream>
#include <vector>

int main(){
    std::vector<int> a(3,4);
    std::cout << a[4] << std::endl;
    std::cout << a.at(4) << std::endl;
    return 0;
}
/* Output:
1766203501
terminate called after throwing an instance of 'std::out_of_range' what():  vector::_M_range_check: __n (which is 4) >= this->size() (which is 3)
*/
```

**Why?**

Checking whether out of bound is not efficient

***

# `std::deque<T>`

Double Ended QUEue

Faster to `push_front` and `pop_front` compared with `vector`

Slower to do other operations (like index) than `vector()`

***

# Which to use?

> “vector is the type of sequence that should be used by default... deque is the data structure of choice when most insertions and deletions take place at the beginning or at the end of the sequence.” 
>
> ​																																		－ C++ ISO Standard (section 23.1.1.2)



***

# How to implement stack and queue?

- Stack: Just limit the functionality of a vector/deque to only allow push_back and pop_back.
- Queue: Just limit the functionality of a deque to only allow push_back and pop_front

Summary: Stack and Queue are container adaptors under the hood that are adapted from the vector.

***

# Why not just use a vector/deque?

Design philosophy of C++:

- Express ideas and intent directly in code.
- Compartmentalize messy constructs.

***

# Change standard container for container adaptor:

Document:

![image-20230114153244828](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230114153244828.png)

Notice: The standard containers `std::vector` (including `std::vector<bool>`), `std::deque` and `std::list` satisfy these requirements. By default, if no container class is specified for a particular stack class instantiation, the standard container `std::deque` is used.







