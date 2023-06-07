[toc]

# iostreams

**Four standard iostreams:**

- `cin`: Standard input stream
- `cout`: Standard output stream (buffered)
- `cerr`: Standard error stream (unbuffered)
- `clog`: Standard error stream (buffered)

***

## `cin` and `cout`

`cin` is an object of istream

```cpp
extern istream cin; // Declared in iostream
```

**Notice:** When enter is pressed the string in console will be sent to buffer. For example, if we input 123456, there will be 7 characters in buffer (123456\n).

***

## Why `>>` with `cin` is a nightmare?

1. `cin` reads the entire line into the buffer but gives you whitespace-separated tokens.
2. Trash in the buffer will make `cin` not prompt the user for input at the right time.
3. When `cin` fails, all future `cin` operations fail too. 

Example: 

```c++
// Input: Avery Wang
string name;
int age;
int rest = 0;
cin >> name;
cin >> age;
cin >> rest;
cout << name << age;
cout << rest;
// Output:
//Avery00
```

**Explanation:** 

1. `cin` prompts for input.
2. "Avery Wang" is input and is sent to buffer.
3. "Avery" is input in name.
4. " " is detected between "Avery" & "Wang".
5. `cin` is not empty so it continues trying to read "Wang" into `age`.
6. The attempt is failed and **fail bit** is on.
7. All rest input is frozen.
8. The initial value of `age` and `rest` is output to console.

***

**Solution:** use `getline` to read the whole line.

```cpp
//Input Avery Wang
string name;
int age;
string response;
getline(cin, name, '\n');
cin >> age;
cout << name << age;
cin >> response;
/*
Console:
Avery Wang
20
Avery Wang20
*/

```

**Notice!!!:** User do not get prompt for `cin >> response`. Because `>>` do not skip the `\n` and `getline` do not skip the `\n` at the beginning. 

**Details:**

<img src="C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230104223946854.png" alt="image-20230104223946854"  />

![image-20230104223553499](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230104223553499.png)

***

**How to fix it?**

Use `cin.ignore`

![image-20230104223757991](C:\Users\Lamfu\AppData\Roaming\Typora\typora-user-images\image-20230104223757991.png)

or use `cin.clear()`, which reset the state bit from fail to good.

***

# Implementing getInteger

```cpp
#include <iostream>
#include <sstream>
using namespace std;
int main(){
    while (true)
    {
        string line; int result; char trash = '\0';
        if (!getline(cin, line)) throw domain_error("error");
        istringstream iss(line);
        if (iss >> result && !(iss >> trash)) 
        {
            cout << result <<endl;
            return result;
        }
        
    } 
    return 0;
}
```

***

## Difference between `cin >>` and `getline()` and `cin.get` and `cin.getline`

- `cin.get()` and `cin.getline()` needs to specify string length to read in.

- `cin >>` and `getline()` needs to specify string length to read in.
***

- `cin >>` stops at specified stop sign (' ' or '\n') and leave the stop sign in buffer.

- `getline()` stops at specified stop sign and will flush the stop sign out of the buffer

***

- `cin >>` ignores the blank spaces and related stuff at the beginning.
- `getline()` does not ignore those black spaces and related stuff at the beginning.



