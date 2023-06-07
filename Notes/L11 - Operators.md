[toc]

# Operator Overloading

![image-20230601155841403](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230601155841403.png)

# Canonical Form

## General Rule of Thumb: `member` vs.  `non-member`

1. Some operators must be implemented as members ( eg. [], (), ->, =) due to C++ semantics.
2. Some must be implemented as non-members (eg. <<, if you are writing class for rhs, not lhs): as friend function
3. If unary operator (eg. ++), implement as member.
4. If binary operator and treats both operands equally (eg. both unchanged) implement as non-member (maybe friend). Examples: +, <.
5. If binary operator and not both equally (changes lhs), implement as member (allows easy access to lhs private members). Examples: +=

# POLA (Principle of Least Astonishment)

> C.160: Design operators primarily to mimic conventional usage.
>
> C.161: Use nonmember functions for symmetric operators.
>
> Always provide all out of a set of related operators.
>
> Example:
>
> If you implement ==, you should implement all of related operators: !=, <, >, <=, >=
>
> ![image-20230601190455506](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230601190455506.png)

# `const` vs. `non-const` version of member function

```cpp 
const vector<string> v1; // Have to implement both [] and const [] version
v1[0]; // Have to use const version of []
```

