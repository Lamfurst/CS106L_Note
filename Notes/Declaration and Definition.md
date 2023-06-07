# There is no limitation on declaration. But there is only one definition in One Translation Unit (header files + source file)!!

# Variables

```cpp
extern int extren_a; // Declaration, tell the current cpp that this variable has been defined in other files
int a; //Definition
int b = 1; //Definition
```

# Functions

```cpp
void func(); // Prototype or Declaration
void func(){...} // Definition
```

# Class

```cpp
class A; // Declaration
class A{}; // Definition
```



# Multiple Definitions in Single C++

**Notice:** This is NEVER allowed!!!!

This is again the:

> **One Definition Rule (ODR): there should be only one definition in one Translation Unit(header files + source file)**

And this is the reason why we need the **include guard** to prevent multiple definition in single cpp.

For example: a.h contains definition for a class. b.h include a.h. main.cpp include both a.h and b.h. Then in main.cpp, there will be two a.h and two definitions for a class. This is not allowed!

# Multiple Definitions in Multiple C++ && Single Definition in Single C++

All Definitions in multiple c++, except for the following three cases, are not allowed !!

1. const object: 因为全局的 const 对象默认是没有 extern 的声明的，所以它只在当前文件中有效。把这样的对象写进头文件中，即使它被包含到其他多个 .cpp 文件中，这个对象也都只在包含它的那个文件中有效，对其他文件来说是不可见的，所以便不会导致多重定义。同时，因为这些 .cpp 文件中的该对象都是从一个头文件中包含进去的，这样也就保证了这些 .cpp 文件中的这个 const 对象的值是相同的，可谓一举两得。同理，static 对象的定义也可以放进头文件。
2. inline functions: 因为inline函数是需要编译器在遇到它的地方根据它的定义把它内联展开的，而并非是普通函数那样可以先声明再链接的（内联函数不会链接），所以编译器就需要在编译时看到内联函数的完整定义才行。如果内联函数像普通函数一样只能定义一次的话，这事儿就难办了。因为在一个文件中还好，我可以把内联函数的定义写在最开始，这样可以保证后面使用的时候都可以见到定义；但是，如果我在其他的文件中还使用到了这个函数那怎么办呢？这几乎没什么太好的解决办法，因此 C++ 规定，内联函数可以在程序中定义多次，只要内联函数在一个 .cpp 文件中只出现一次，并且在所有的 .cpp 文件中，这个内联函数的定义是一样的，就能通过编译。那么显然，把内联函数的定义放进一个头文件中是非常明智的做法。
3. class: 因为在程序中创建一个类的对象时，编译器只有在这个类的定义完全可见的情况下，才能知道这个类的对象应该如何布局，所以，关于类的定义的要求，跟内联函数是基本一样的。所以把类的定义放进头文件，在使用到这个类的 .cpp 文件中去包含这个头文件，是一个很好的做法。在这里，值得一提的是，类的定义中包含着数据成员和函数成员。数据成员是要等到具体的对象被创建时才会被定义（分配空间），但函数成员却是需要在一开始就被定义的，这也就是我们通常所说的类的实现。一般，我们的做法是，把类的定义放在头文件中，而把函数成员的实现代码放在一个 .cpp 文件中。这是可以的，也是很好的办法。不过，还有另一种办法。那就是直接把函数成员的实现代码也写进类定义里面。在 C++ 的类中，如果函数成员在类的定义体中被定义，那么编译器会视这个函数为内联的。因此，把函数成员的定义写进类定义体，一起放进头文件中，是合法的。注意一下，如果把函数成员的定义写在类定义的头文件中，而没有写进类定义中，这是不合法的，因为这个函数成员此时就不是内联的了。一旦头文件被两个或两个以上的 .cpp 文件包含，这个函数成员就被重定义了。

# Summary:

The **one-definition rule** (3.2, [basic.def.odr]) applies differently to classes and functions:

> 1 - No translation unit shall contain more than one definition of any variable, function, class type, enumeration type, or template.
>
> [...]
>
> 4 - Every program shall contain exactly one definition of every non-inline function or variable that is odr-used in that program [...]

