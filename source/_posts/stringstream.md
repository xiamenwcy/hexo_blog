title: 关于stringstream 的一些坑
date: 2016-10-16 18:06:58
categories: C/C++
toc: false
---
**关键点一：**<font color="#FF0000"> **同一个stringstream对象来多次处理数据，每次使用前，使用stream.str("");保证数据已清空。**</font>
**例如：**
```
std::stringstream ss;
string result;
ss << 1;
ss>>result;
//必须牢记使用stringstream两次输入，必须使用前清空
ss.clear();
ss.str("");
ss << 2;       
```
又或者参看下面这段程序：
```
#include <cstdlib>
#include <iostream>
#include <sstream>
using namespace std;
/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
int main(int argc, char * argv[])
{
	std::stringstream stream;
	string str;
	while (1)
	{
		//clear()，这个名字让很多人想当然地认为它会清除流的内容。 
		//实际上，它并不清空任何内容，它只是重置了流的状态标志而已！
		stream.clear();
		// 去掉下面这行注释，清空stringstream的缓冲，每次循环内存消耗将不再增加!
		//stream.str("");      
		stream << "sdfsdfdsfsadfsdafsdfsdgsdgsdgsadgdsgsdagasdgsdagsadgsdgsgdsagsadgs ";
		stream >> str;
		// 去掉下面两行注释，看看每次循环，你的内存消耗增加了多少！
		cout<<"Size of stream = "<<stream.str().length()<<endl;
		system("PAUSE");
	}
	system("PAUSE ");
	return EXIT_SUCCESS;
}
```
我们发现运行程序前打开任务管理器，过不了几十秒，所有的内存都将被耗尽！

而把stream.str(""); 那一行的注释去掉，再运行程序，内存就正常了。

看来stringstream似乎不打算主动释放内存(或许是为了提高效率)，但如果你要在程序中用同一个流，反复读写大量的数据，将会造成大量的内存消耗，因些这时候，需要适时地清除一下缓冲 (用 stream.str("") )。

另外不要企图用 stream.str().resize(0)，或 stream.str().clear() 来清除缓冲，使用它们似乎可以让stringstream的内存消耗不要增长得那么快，但仍然不能达到清除stringstream缓冲的效果(不信做个实验就知道了，内存的消耗还在缓慢的增长！)
<!--more-->

**关键点二：**<font color="#FF0000"> **stringstream处理字符串时，会跳过空白制表符**</font>
**例如：**
```
#include <cstdlib>
#include <iostream>
#include <sstream>
using namespace std;
/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
int main(int argc, char * argv[])
{
	std::stringstream ss;
	string result;
	ss << "hello world";
	ss >> result;
	cout << result;

	return EXIT_SUCCESS;
}
```
Result:
```
hello请按任意键继续. . .
```
发现并没有将world输出去。这表明stringstream会跳过空白符。为了避免这种情况，可以使用两种方法：
- 使用getline :
```
std::stringstream ss;
string result;
ss << "hello world";
getline(ss, result);
cout << result;
```
- 使用.str() :
```
std::stringstream ss;
string result;
ss << "hello world";
result = ss.str();
cout << result;
```
两者都可以正常输出。
**关键点三：**<font color="#FF0000"> **stringstream关于std::noskipws及std::ws的使用**</font>
std::noskipws ：不要跳过空格
std::ws：跳过空格

-  std::noskipws
关于上面第二条，有的人建议使用std::noskipws避免跳过空格，即：
```
	std::stringstream ss;
	ss << std::noskipws;
	string result;
	ss << "hello world";
	ss >> result;
	cout << result;
```
实际发现并不起作用，因为std::noskipws的作用是当流以空格开头时，会起作用。
即如果改为：
```
	std::stringstream ss;
	ss << std::noskipws;
	string result;
	ss << " hello world";
	ss >> result;
	cout << result;
```
将输出空格。注释掉std::noskipws，将会输出
```
hello请按任意键继续. . 
```
- std::ws,可以使得using getline时去除前面的空格
请看如下代码：
```
std::stringstream ss;
string result;
ss << " a b c" ;
//ss >> std::ws;
getline(ss, result);
cout << result;
```
将会输出，注意a前面的空格仍在。以'~'代替空格说明
```
~a b c请按任意键继续. . .
```
去掉`ss >> std::ws;`的注释后，变成：
```
a b c请按任意键继续. . .
```
a前面的空格变没了。


# 参考文献：
1. [stringstream doesn't accept white space?][1]
2. [Skip whitespaces with getline][2]
3. [关于stringstream的格式化的注意事项(转载)][3]
4. [C++ 输入输出流][4]


  [1]: http://stackoverflow.com/questions/16935026/stringstream-doesnt-accept-white-space
  [2]: http://stackoverflow.com/questions/20045726/skip-whitespaces-with-getline
  [3]: http://blog.csdn.net/superql/article/details/7826643
  [4]: https://github.com/xuelangZF/CS_Offer/blob/master/C++/InputOutput.md
