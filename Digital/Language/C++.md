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
