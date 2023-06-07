[toc]

# Review of Classes

# Everything `const`

`const iterator`

```cpp
const vector<int>::iterator itr = v.begin(); // Similar to int* const itr
++itr; // does not compile
*itr = 15; // compile

vector<int>::const_iterator itr = v.begin();
++itr; // compile
*itr = 15; // does not compile
```

`const` on objects

Guarantee that the object won't change

`const member function`

Guarantee that the function won't call anything but `const` functions.
