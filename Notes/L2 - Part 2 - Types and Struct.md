[toc]

# Interlude: C++ types

**Notes:**

- STL types can be long. How to fix it?

  **Alias and auto!**

  Example:

  ```c++
  std::unordered_map<forward_list<Student>, unordered_set>::const_iterator 
      begin = studentMap.cbegin();
  ```

  Instead, using Alias:

  ```c++
  using map_iter = std::unordered_map<forward_list<Student>, unordered_set>::const_iterator;
  map_iter begin = studentMap.cbegin();
  ```

  or

  ```cpp
  auto begin = studentMap.cbegin();
  ```

***

## Good Guidelines to Follow for `auto`

- Use it when the types is clear from context.

- Use it when the exact type is unimportant.

- Don't use it when it obviously hurts readability.

  Example:

  ```cpp
  auto spliceString(const string& s);
  //Hard to tell the exact return type.
  ```

***

# How to return two values?

**C++11**

![image-20230104234310590](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230104234310590.png)

**C++17**

![image-20230104234348096](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230104234348096.png)



---

# How to return multiple values?

## Use Struct

```cpp
Struct PriceRange{
    int min;
    int max;
}
PriceRange findPriceRange(int dist){
    int min = static_cast<int>(dist * 0.08 + 100);
    int max = static_cast<int>(dist * 0.36 + 750);
    return PriceRange{min, max};
}

int main(){
    int dist = 6452;
    PriceRange p = findPriceRange(dist);
    cout << "You can find prices between:"
        << p.min << " and " << p.max;
}

// We can also use structured binding here
int main(){
    int dist = 6452;
    auto [min, max] = findPriceRange(dist);
    cout << "You can find prices between:"
        << min << " and " << max;
}
// The order the binding occurs is the same order as the variables are laid in the struct
```

**Notice:** `p.min` here is a a reference to the member in struct

## Use Tuple Object

Example:

```cpp
#include <tuple>

std::tuple<int, double, char> example_function() {
  return std::make_tuple(1, 3.14, 'a');
}

int main() {
  auto result = example_function();
  int x = std::get<0>(result);
  double y = std::get<1>(result);
  char z = std::get<2>(result);
}
```

# Parameters and Return Value Guideline for Modern C++

![C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/param-passing-advanced.png)

---

# Initialization

C++ has a lots of ways to initialize a variable (26+ ways!!!)

To solve this, we can use uniform initialization:

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

struct Time{
    int hour;
    int minute;
};
struct Course{
    string course;
    Time start; Time end;
    vector<string> instructors;
};
int main(){
    //Uniform Initialization
    vector<int> vec{3,1,4,1,5,9};
    Course now {{"CS106L"}, {15,30}, {16,30}, {"Wang", "Zeng"}};
}
```

```cpp
// ctor means constructor
// ctor for vector when using vector<int> vec{3,1,4,1,5,9} to initialize
vector::vector(initializer_list<T> init);
// The advantage of initializer_list is that we don't need to specify the size of the input parameters.
// For example
#include <initializer_list>
#include <iostream>

void print_elements(std::initializer_list<int> lst) {
  //We don't need to specify the length of lst here
  for (int x : lst) {
    std::cout << x << " ";
  }
  std::cout << std::endl;
}

int main() {
  print_elements({1, 2, 3});  // prints "1 2 3"
  return 0;
}
```

***

