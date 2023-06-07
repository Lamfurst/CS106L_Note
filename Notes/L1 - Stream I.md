[toc]

# Overview + String Stream

***

## Procedure of I/O data

Files, Pipelines		$\Longleftrightarrow$					"3.14"				$\Longleftrightarrow$				3.14

external source						string representation					double in program

---

## Stringstream:

## Output stream:

Basic Use:

```c++
#include <sstream>
#include <iostream>
using namespace std;
int main(){
    ostringstream oss("Ito En Green Tea ");
	cout << oss.string() << endl;
    return 0
}
```
**Output: Ito En Green Tea**

***

## What will happen if we input stream into stream?

```c++
#include <sstream>
#include <iostream>
using namespace std;
int main(){
    ostringstream oss("Ito En Green Tea ");
	// cout << oss.string() << endl;
	oss << 16.9 << "Ounce "
	cout << oss.string() << endl;
    return 0
}
```
**Output: **

**16.9 Ounce n Tea**

Explanation: << is an operator of oss(ostringstream). When no additional parameter is announced, the start point will always be the beginning of the stream and the input string will rewrite the existed string.

***

## How to add string at the end?

If we always want to add the string at the end of the existed string, we should use `ostringstream::ate` when initializing the ostringstream. For example ``ostringstream oss("Ito En Green Tea ", ostringstream::ate)``. (`ate`means: at end)

```c++
#include <sstream>
#include <iostream>
using namespace std;
int main(){
    ostringstream oss("Ito En Green Tea ", ostringstream::ate);
	// cout << oss.string() << endl;
	oss << 16.9 << "Ounce ";
	cout << oss.string() << endl;
    return 0
}
```
**Output: **

**Ito En Green Tea 16.9 Ounce  **

***

**Notice: Once we have input something to the stream, the input position will be at the end of the already input string when we input new string**

Example:
```c++
#include <sstream>
#include <iostream>
using namespace std;
int main(){
    ostringstream oss("Ito En Green Tea ", ostringstream::ate);
	// cout << oss.string() << endl;
	oss << 16.9 << "Ounce ";
	cout << oss.string() << endl;
	oss << "(Pack of " << 12 << ")";
	cout << oss.string() << endl;
    return 0
}
```
**Output: **

**16.9 Ounce n Tea **
**16.9 Ounce (Pack of 12)**

***

## Input stream:
```c++
#include <sstream>
#include <iostream>
using namespace std;
int main(){
    ostringstream oss("Ito En Green Tea ", ostringstream::ate);
	// cout << oss.string() << endl;
	oss << 16.9 << "Ounce ";
	cout << oss.string() << endl;
	oss << "(Pack of " << 12 << ")";
	cout << oss.string() << endl;
	istringstream iss(oss.str());
	double amount;
	string unit;
	
	iss >> amount >> unit;
    cout << amount / 2 << " " <<unit;
    return 0
}
```

**Output: **
**16.9 Ounce n Tea **
**16.9 Ounce (Pack of 12)**
**8.45 Ounce**

***

## How to change the last position in buffer?

Use `tellp()` and `seekp()`

```c++
#include <sstream>
#include <iostream>
using namespace std;
int main(){
    ostringstream oss("Ito En Green Tea ");
    oss << 16.9;
    fpos pos = oss.tellp() + streamoff(3);
    // fpos: a special for the postition of the current buffer position
    // tellp(): return the position of the last character in buffer
    // streamoff(): a special offset for fpos
    oss.seekp(pos);
    // seekp() set the postion in buffer to the expected positon
	cout << oss.string() << endl;
    return 0
}
```

**Output:**

**16.9En Black Tea**
***

# State Bits: Indicate the state of the stream

***

- **Good bit: ** Ready for read/write
- **Fail bit:** Previous operation failed, all future operation frozen
- **EOF bit:** Previous operation reached the end of buffer content
- **bad bit:** External error, likely irrecoverable

***

## Common reasons why that bit is on:

- **Good bit: ** Nothing usual, on when other bits are off
- **Fail bit:** Type mismatch, file can't be opened, `seekg` failed.
- **EOF bit:** Reached the end of the buffer
- **bad bit:** Could not move characters to buffer from external source. (e.g. the file you are reading from suddenly is deleted)

***

**Notice:**

- Good and Bad are not opposites. (e.g. type mismatch)
  - Good = False && Bad = False
- Good and Fail are not opposites (e.g. end of file)
  - Good =  False && Fail = False
- EOF and Fail are normally the ones that are checked：
  - Example: `if(iss.fail() || !iss.eof()) throw domain_error("transfer error...")`

Example:

```cpp
void printStateBits(istream& s){
    cout << (s.good() ? "G" : "-"); //Ternary Operator
    cout << (s.fail() ? "F" : "-");
    cout << (s.eof() ? "E" : "-");
    cout << (s.bad() ? "B" : "-");
    cout << endl;
}
istringstream iss1("5.2");
istringstream iss2("");
istringstream iss3("lol");
printStateBits(iss1);
printStateBits(iss2);
printStateBits(iss3);
```

**Output:**

**G---**

**-FE-**

**-F--**

***

# Use return of '>>' as a short cut to State Bit

```cpp
iss >> remain;
if(iss.fail())
{
    //report error
}

if(!(iss >> remain))
{
    //report error
}  
```

**Notice:** The return type of `a >> b` when `a` is an `istringstream` object is `istringstream&`. This allows you to chain multiple input operations together using the `>>` operator. 

***

# When to use stringstream?

- Simplify "/./a/b/.." to "/a"

  - ```cpp
    #include <sstream>
    #include <iostream>
    #include <istream>
    
    using namespace std;
    
    int main(){
        istringstream iss("/a/b/c/d");
        string rec;
        iss.ignore();
        while(getline(iss, rec, '/'))
        {
            cout << rec << endl;
        } 
    }
    ```

- Formatting input/output

  - uppercase, hex, and other stream manipulators

  - ```cpp
    // Upper case example
    #include <sstream>
    #include <iomanip>
    
    int main() {
      std::stringstream ss;
      ss.setf(std::ios::uppercase);  // set uppercase flag
    
      // insert a lowercase string
      ss << "hello";
      std::string str = ss.str();  // str is now "HELLO"
    
      return 0;
    }
    ```

  - ```cpp
    // Hex example
    #include <sstream>
    #include <iomanip>
    
    int main() {
      std::stringstream ss;
      ss << std::setw(8) << std::setfill('0') << std::hex;  // set hexadecimal formatting
    
      // insert an integer
      ss << 255;
      std::string str = ss.str();  // str is now "0000ff"
    
      return 0;
    }
    ```

- Parsing different types

  - stringTointeger() from previous lectures







