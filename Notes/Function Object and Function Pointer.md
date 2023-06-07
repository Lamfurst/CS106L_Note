[toc]

# Function Object

> A function object is *any object for which the function call operator is defined*.

# Function Pointer

Example

```c++
typedef int (* t_func)(void);//定义了一个新类型 t_func; 
int (*func)(void); // 变量声明语句(指针函数声明):  
//.... 


t_func func2 = func; //func2和func是相同类型，这就好理解加上"typedef"产生的变化了。

```



# Conversion Operator

Example:

```cpp
class IntClass
{
    public:
    	IntClass(int inVal):val(inVal)
    	{ ; }
    operator int()
    	{ 
    		return val;
    	};
    operator bool()
    	{ 
    		return val == 0?false:true;
    	};
    private:
    	int val;
};

// Conversion Operator can make this clss work as bool or int when needed

int main()
{
	IntClass ic1(13);
	IntClass ic2(0);
	int x;
	x = 4 + ic1; // Make IntClass work as int
	cout << "x: " << x << endl;
	cout << "IC1 WAS " << 
		(ic1?"TRUE":"FALSE") << endl; // Make IntClass work as bool
	cout << "IC2 WAS " << 
		(ic2?"TRUE":"FALSE") << endl;
	return 0;
}

```



