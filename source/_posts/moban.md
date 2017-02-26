title: 模板函数的使用
date: 2016-10-17 13:59:33
categories: C/C++
toc: false
---
1. <font color="#FF0000"> **模板函数应该将声明与定义放在一起**</font>
看如下例题：
```
//tem.h
#ifndef _TEM_H
#define _TEM_H
template<typename T> T add(T a, T b);
//{
//return a+b;
//}
#endif

//tem.cpp
#include "tem.h"
template <typename T> T add(T a, T b)
{ 
	return a + b;
}
template int add(int, int);//实例化定义，必须放在模板定义的后面

//main.cpp
#include <iostream>
#include "tem.h"
using namespace std;
int main()
{
	cout << add(1, 2);
	return 0;
}
```
对普通函数来说，声明放在头文件中，定义放在源文件中，其它的地方要使用该函数时，仅需要包含头文件即可，因为编译器编译时是以一个源文件作为单元编译的，当它遇到不在本文件中定义的函数时，若能够找到其声明，则会将此符号放在本编译单元的外部符号表中，链接的时候自然就可以找到该符号的定义了。

而对模板函数来说，首先明确，模板函数是在编译器遇到**使用模板的代码**时才将模板函数实例化的。若将模板函数声明放在tem.h，模板定义放在tem.cpp,在main.cpp中包含头文件，调用add,按道理说应该实例化int add(int,int)函数，即生成add函数的相应代码，但是此时仅有声明，找不到定义，因此此时，它只会实例化函数的符号，并不会实例化函数的实现，<font color="#FF0000">即这个时候，在main.o编译单元内，它只是将add函数作为一个外部符号，这就是与普通函数的区别，对普通函数来说，此时的add函数已经由编译器生成相应的代码了，而对模板函数来说，此时并没有生成add函数对应的代码。</font>此时编译main.cpp单元不会报错，但链接就会出现add函数未定义的错误。

因此，我们可以通过显式的实例化定义，即通过加上语句`temmplate int add(int,int)`,编译器看到此语句将会生成add方法的int版本，这样的话，再链接就不会报错了。此外，这样做通常也能够<font color="#FF0000">提高编译的效率</font>。试想，如果在tem.h文件内定义模板，假如有三个源文件均包含了该头文件且均使用了模板（假定均调用了add模板的int版本），则在这三个源文件内必然都会生成add函数的实例。显然效率不高。而如果像上面那样使用该模板，则只会在tem.cpp文件中实例化。

同理对于类模板和类模板函数亦如此。<!--more-->
2. 下面列出几种正确的做法。

-  **声明与实现放在一起**
```
#include <limits>
#include <iostream>
#include <algorithm>
class A
{
public:
	A();
	~A();
	template<class T>
	bool almost_equal(T x, T y) //成员函数
	{
		// the machine epsilon has to be scaled to the magnitude of the values used
		// and multiplied by the desired precision in ULPs (units in the last place)
		return std::abs(x - y) < std::numeric_limits<T>::epsilon() * std::abs(x + y) * 2
			// unless the result is subnormal
			|| std::abs(x - y) < std::numeric_limits<T>::min();
	};
	
private:
	bitmap_image* img;
};

//全局函数
template<class T>
bool almost_equal(T x, T y)
{
	// the machine epsilon has to be scaled to the magnitude of the values used
	// and multiplied by the desired precision in ULPs (units in the last place)
	return std::abs(x - y) < std::numeric_limits<T>::epsilon() * std::abs(x + y) * 2
		// unless the result is subnormal
		|| std::abs(x - y) < std::numeric_limits<T>::min();
};
```
这种写法省事，而且适合于多种类型的模板化，很方便。但是如果在多个源文件中仅仅只使用一种实例化，那么这种方法不高效，因为进行了多次编译，下面这种方法将会简化，只进行一次编译。

- **声明与实现分开，但增加显式的模板化**

```
//A.h
#include <limits>
#include <iostream>
#include <algorithm>
class A
{
public:
	template<class T>
	bool almost_equal(T x, T y); //成员函数
};

//全局函数
template<class T>
bool almost_equal(T x, T y);


//A.cpp
#include "A.h"
template<class T>
bool A::almost_equal(T x, T y)
{
	// the machine epsilon has to be scaled to the magnitude of the values used
	// and multiplied by the desired precision in ULPs (units in the last place)
	return std::abs(x - y) < std::numeric_limits<T>::epsilon() * std::abs(x + y) * 2
		// unless the result is subnormal
		|| std::abs(x - y) < std::numeric_limits<T>::min();
};
template bool A::almost_equal(double, double); //显式化

template<class T>
bool almost_equal(T x, T y)
{
	// the machine epsilon has to be scaled to the magnitude of the values used
	// and multiplied by the desired precision in ULPs (units in the last place)
	return std::abs(x - y) < std::numeric_limits<T>::epsilon() * std::abs(x + y) * 2
		// unless the result is subnormal
		|| std::abs(x - y) < std::numeric_limits<T>::min();
};
template bool almost_equal(int, int);//显式化

```
使用时，如下：
```
int main()
{
	A a;
	bool s1=a.almost_equal(1,2); //错误，编译器推导出为int，但是实际上面显式化的是double，所以错误，但是改为下面的形式即可正确。
	 s1= a.almost_equal<double>(1,2);//正确，强制转为double
	bool s = almost_equal(1., 2); //错误
	s= almost_equal<int>(1., 2); //正确，强制转为int
	return 0;
}

```






# 参考文献
1. [关于模板函数声明与定义的问题][1]


  [1]: http://blog.csdn.net/cllcsy/article/details/50485324

