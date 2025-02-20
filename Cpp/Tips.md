*2025-02-20*

## C++ 小技巧

### C++ 在指定内容地址创建对象

**语法**
```cpp
new (address) type(arguments);
```
**示例**
```cpp
#include <iostream>

class Example {
public:
    Example() {
        std::cout << "Example object constructed." << std::endl;
    }
    ~Example() {
        std::cout << "Example object destroyed." << std::endl;
    }
    void recreate() {
        // 使用定位 new 在当前对象的内存位置重新构造对象
        new (this) Example();
    }
};

int main() {
    Example* obj = new Example();
    // 手动调用析构函数销毁对象，但不释放内存
    obj->~Example();
    // 使用定位 new 重新构造对象
    obj->recreate();
    // 再次手动调用析构函数
    obj->~Example();
    // 释放内存
    delete obj;
    return 0;
}
```