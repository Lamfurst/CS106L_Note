[toc]

# `std::multimap`

Maps store unique keys. Sometimes we want to allow the map to have the same key pointing to different values

Example:

```cpp
multimap<int, int> myMMap;
myMMap.insert(make_pair(3, 3));
myMMap.insert({3, 12}); // shorter syntax
cout << myMMap.count(3) << endl; // prints 2
```



