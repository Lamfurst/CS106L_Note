[toc]

# `std::sort`

```cpp
std::vector<Course> readCourses() {
    std::vector<Course> v = {{"CS 106A", 4.4337}, {"CS 106B", 4.4025},
        {"CS 107", 4.6912}, {"CS 103", 4.0532},
        {"CS 109", 4.6062}, {"CS 110", 4.343},
        {"Math 51", 3.6119}, {"Math 52", 4.325},
        {"Math 53", 4.3111}, {"Econ 1", 4.2552},
           {"Anthro 3", 3.71}, {"Educ 342", 4.55},
           {"Chem 33", 3.50}, {"German 132", 4.83},
           {"Econ 137", 4.84}, {"CS 251", 4.24},
           {"TAPS 103", 4.79}, {"Music 21", 4.37},
           {"English 10A", 4.41}};
    std::random_shuffle(v.begin(), v.end());
    return v;
}
auto courses = readCourses();

// We need to define what is the meaning of c1 < c2 (smaller)
// Lambda function here
auto compareRating = [](const Course& c1,const Course& c2)
{
    return c1.rating < c2.rating;
}


std::sort(courses.begin(), courses.end, compareRating);
int size = course.size() / 2
Course median = courses[size]; // O(nlogn)
```

# `std::nth_element`

```cpp
size_t size = courses.size();
Course median = *std::nth_element(classes.begin(), classes.end(), size/2, compAvg); // O(N)
```

# `std::stable_ partition`

- The true ones are at before, the false ones are at back
- The internal order is kept

# `std::copy_if`

```cpp
std::vector<Course> csCourses;
std::copy_if(courses.begin(), course.end(), 
             back_inserter(csCourses), isDep);
// We have to use back_inserter here
// It is a iterator adaptor
// It can extend the end of the vector if csCourses is not long enough
// Because copy_if is not member function of vector
// So we have to use back_inserter to extend the vector
```

# `std::remove_if`

- Does not remove the element, becuase this is not a member function of vector

- Move the element (need to remove) to the end of the vector

- And return iterator pointing to where the trash starts

- Need to call erase to actually remove

- Erase-remove idiom:

- ```cpp
  v.erase(
      std::remove_if(v.begin(), v.end(), pred),
      v.end()
  ); 
  ```

# `std::transform`

Switch string to all lower case

```cpp
std::string line{"AbcD"}
std::transform(line.begin(), line.end(), line.begin(), tolower);
```

