[toc]

# Generic Programming

> **Generic programming** is a style of [computer programming](https://en.wikipedia.org/wiki/Computer_programming) in which [algorithms](https://en.wikipedia.org/wiki/Algorithm) are written in terms of [types](https://en.wikipedia.org/wiki/Data_type) *to-be-specified-later* that are then *instantiated* when needed for specific types provided as [parameters](https://en.wikipedia.org/wiki/Parameter_(computer_programming)).	-- Wikipedia

***

# Template Function

Example:

```cpp
template <typename T>
pair<T, T> my_minmax(T a, T b){
    if (a < b) return {a, b};
    else return {b, a};
}
```

Example:

```cpp
#include <iostream>
#include <sstream>
using namespace std;

template<typename T>
T getTypeName();

int main(){
    auto a = getTypeName<float>();
    cout << "Output is:" << a / 2 <<endl;
}

template <typename T>
T getTypeName(){
    while (true)
    {
        string line; T result; char trash = '\0';
        if (!getline(cin, line)) throw domain_error("error");
        istringstream iss(line);
        if (iss >> result && !(iss >> trash)) 
        {
            cout << "Input is:" << result <<endl;
            return result;
        }
        
    } 
}
```

***

## Explicit Instantiation: Specify the type T

```cpp
template <typename T>
pair<T, T> my_minmax(T a, T b){
    if (a < b) return {a, b};
    else return {b, a};
}

int main(){
    auto [min1, max1] = my_minmax<double>{4.2, 8.8};
}
```

***

## Implicit Instantiation:

**Notice:**

```c++
auto [min1, max1] = my_minmax("Avery", "Anna");
// This does not do the thing as expexted
// Because with inplicit instantiation this
// are default recognized as "cstring", 
// which does not have overloaded compare function.

// Instead, we should explicitly instantiation this
auto [min1, max1] = my_minmax<string>("Avery", "Anna");
// string has overloaded compare function.
```



# Concept Lifting

> Looking at the assumptions you place on the parameters,  and questioning if they are really necessary.

![image-20230116213900876](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230116213900876.png)

![image-20230116214325236](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230116214325236.png)

![image-20230116214339267](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230116214339267.png)

![image-20230116214413720](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230116214413720.png)

![image-20230116214427273](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230116214427273.png)

**This does not work! Because list does not have index operator!**

**Use iterator instead!**

![image-20230116214613085](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230116214613085.png)

![image-20230116214634299](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230116214634299.png)

![image-20230116214746362](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230116214746362.png)

**Notice:** We name `typename` to `InputIterator` because we do not want to use other iterators such as `Random Access Iterator`. Because not all containers have random access.

***

