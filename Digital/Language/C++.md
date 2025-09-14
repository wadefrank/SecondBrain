# 静态成员变量

## 静态成员变量的初始化

**静态成员变量的初始化通常在类的实现文件（.cpp 文件）中进行**，这是因为静态成员变量只需要被初始化一次，而不是每次创建类的实例时都进行初始化。这样做可以确保静态成员变量只有一个实例，并且在程序中的所有编译单元中保持一致。

如果在类的头文件中进行静态成员变量的初始化，而且在多个编译单元中包含了这个头文件，就会导致静态成员变量的多重定义，从而引发链接错误。

因此，为了避免这种问题，通常将静态成员变量的初始化放在类的实现文件（.cpp 文件）中。这样可以确保静态成员变量只被初始化一次，而且只在类的实现文件中被初始化，从而避免了多重定义的问题。

# 统计耗时

## chrono库

chrono库是 C++ 标准库的一部分，需要C++11以上，是进行时间测量的首选。

```C++
#include <iostream>
#include <chrono>
#include <thread> // 用于 sleep

int main() {
    // 记录开始时间点
    auto start = std::chrono::steady_clock::now();
    
    // 模拟耗时操作（实际应用中替换为你的代码）
    std::this_thread::sleep_for(std::chrono::milliseconds(150));
    
    // 记录结束时间点
    auto end = std::chrono::steady_clock::now();
    
    // 计算时间差（duration）
    auto duration = end - start;
    
	// 转换为不同时间单位 
	auto ns = std::chrono::duration_cast<std::chrono::nanoseconds>(duration);
	auto us = std::chrono::duration_cast<std::chrono::microseconds>(duration);
	auto ms = std::chrono::duration_cast<std::chrono::milliseconds>(duration);
	auto sec = std::chrono::duration_cast<std::chrono::seconds>(duration); 
	// 输出不同单位的耗时
	std::cout << "纳秒: " << ns.count() << " ns\n";
	std::cout << "微秒: " << us.count() << " μs\n";
	std::cout << "毫秒: " << ms.count() << " ms\n";
	std::cout << "秒: " << sec.count() << " s\n";
}
```
