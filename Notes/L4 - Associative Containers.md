[toc]

# Associative Containers

- Have no idea of a sequence
- Data is accessed using the `key` instead of `indexes`
- Includes:
  - `std::map<T1, T2>`
  - `std::set<T>`
  - `std::unordered_map<T1, T2>`
  - `std::unordered_set<T>`
- Differences: 
  - Map/set: keys in sorted order, faster to iterate through a range of elements
  - Unordered map/set: faster to access individual elements by key

***

# `std::map`

## Difference between `[]` and `.get()`:

- `.get(key)`: Search through the whole map, if `key` does not exist, throw an error
- `[]`: Search through the whole map, if `key` does not exist, create pair with default value

## Check whether a key is contained in map?

Use `.count(key)`

```cpp
std::map<char, int> frequencyMap;
frequencyMap.count(key);
// Returns 0 when not contained 
// Returns 1 when key is contained
```

Example Code:

> Achieve a simple function where you can 

***

# `std::set`

A specific case of a map that does not have a value

***

# Iterators

> In C++, an iterator is an object that can be used to traverse through the elements of a container, such as an array, a vector, a list, or a set. Iterators act as a pointer to the elements of a container, allowing you to access the elements using a consistent and generic interface. 
>
> ​																							---ChatGPT

Examples:

Using `while` loop:

```cpp
#include<iostream>
#include<set>

using namespace std;
int main(){
    set<int> a{1,2,7,4,2};
    auto iter = a.begin();
    while(iter != a.end())
    {
        cout<< *iter << endl;
        ++iter;
    }
    return 0;
}
// Output:
// 1
// 2
// 4
// 7
```

Using `for` loop:

```cpp
#include<iostream>
#include<set>

using namespace std;
int main(){
    set<int> a{1,2,7,4,2};
    set<int>::iterator iter;
    for(iter = a.begin(); iter!= a.end(); iter++)
    {
        cout << *iter << endl;
    }
}
// Output:
// 1
// 2
// 4
// 7
```

***

```cpp
#include<iostream>
#include<set>
#include<string>
#include<map>

using namespace std;
int main(){
    map<int, string> a{{2,"Monday"}, {1, "Tuesday"}, {3, "Wednesday"}};
    map<int, string>::iterator iter = a.begin();
    while(iter != a.end())
    {
        // (*iter) is actually a pair under the hood
        cout << (*iter).second << endl;
        ++iter;
    }
    return 0;
}
/* Output:
Tuesday
Monday
Wednesday
*/
```



***

# Why iterator?

Go through different containers in a standardized way.

***

# Four essential iterator operations:

- **Create** iterator
  - `std::set<int>::iterator iter = mySet.begin();`
- **Dereference** iterator to read value currently pointed to
  - `int val = *iter;`
- **Advance** iterator
  - `iter++; or ++iter`
- **Compare** against another iterator (especially .end() iterator)
  - `if(iter == mySet.end()) return;`

**Take away:** Iterator provides a guaranteed interface to iterate any container. Not matter `unordered_map`, `map`, or `vector`, you name it!

***

# Map Iterator

**Notice:** The iterator of map container points to the `std::pair<T, T>`;

Example:

```cpp
#include<iostream>
#include<set>
#include<string>
#include<map>

using namespace std;
int main(){
    map<int, string> a{{2,"Monday"}, {1, "Tuesday"}, {3, "Wednesday"}};
    map<int, string>::iterator iter = a.begin();
    while(iter != a.end())
    {
        cout << (*iter).second << endl;
        ++iter;
    }
    return 0;
}
```



***

# More on iterator!

## Use iterator in `sort` and `find`.

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

void printVec(const std::vector<int>& vec)
{
    for(auto elem:vec)
    {
        std::cout << elem << " ";
    }
    std::cout << std:: endl;
}

int main(){
    std::vector<int> a{3,1,4,1,5,9,2,6};
    std::cout << "Before" << std::endl;
    printVec(a);
    std::cout << "After" << std::endl;
    std::sort(a.begin(), a.end());
    printVec(a);

    const int numToFind = 6;
    auto iter = std::find(a.begin(), a.end(), numToFind);
    if(iter == a.end()) std::cout << "Element Not Found" << std::endl;
    else std::cout << "Element Found" << std::endl;
    return 0;
}

// Output:
Before
3 1 4 1 5 9 2 6
After
1 1 2 3 4 5 6 9
Element Found
```

**Notice:** The `a.end()` does not point to the last element. Instead, it points to the element past `a.end()`.

**Notice:** `.count()` call `find` under the hood. So it is actually faster to call `find` than `.count()`.

***

## Lower Bound & Upper Bound

`lower_bound:` Returns iterator to first element in the given range which is `EQUAL_TO or Greater than val`.

`upper_bound:` Returns iterator to first element in the given range which is `Greater than val`.

Example:

```cpp
#include <iostream>
#include <algorithm>
#include <map>

using namespace std;
int main(){
    map<int, string> a{{5,"Friday"}, {3,"Wed"}, {4,"Thurs"}};
    map<int, string>::iterator iter = a.lower_bound(3);
    map<int, string>::iterator end = a.upper_bound(5);

    for(; iter != end; iter++)
    {
        cout << (*iter).first << ":" << (*iter).second << endl;
    }
    return 0;
}
// Output:
3:Wed
4:Thurs
5:Friday
```

**A quick table for the ranges:**

![image-20230115161557419](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230115161557419.png)

***

# Range Based `for` Loop:

![image-20230115161806566](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230115161806566.png)

***

# Iterator Types

There are different types of Iterators:

Example:

```cpp
std::deque<int> d(13);
auto some_iter = d.begin() + 3; // This is allowed
//////////////////////////////////////////////////
std::list<int> myList(10);
auto some_iter = myList.begin() + 3; // This is NOT allowed!!
```

***

# Five iterator types:

![image-20230115164722350](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230115164722350.png)

**Notice:** The iterator on the right is a more powerful iterator compared with the ones on the left.

- All iterators share a few common traits:
  - Can be created from existing iterators
  - Can be advanced using ++
  - Can be compared with == and !=
- **Input:** For sequential, single-pass **input**. Read only! Can only be dereferenced on **Right** side of expression.
  - Used in `find` and `count`
  - Input Streams
- **Output:** For sequential, single-pass **output**. Write only! Can only be dereferenced on **left** side of expression.
  - Used in `copy`.
  - Output streams
- **Forward:** Can read from and write to.
  - Used in `replace`.
  - `std::forward_list` (singly-linked list)
- **Bidirectional:** Same as forward, except that can also go backwards with `--`
  - Used in `reverse`.
  - `std::map, std::set`
  - `std::list`
- **Random Access:** Same as **Bidirectional**, except that can be incremented or decremented by arbitrary amounts using + and -.
  - `std::vector, std::deque, std::string`
  - Pointers!

***

