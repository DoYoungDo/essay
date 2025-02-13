# 标准C++ 定义一个函数类型

> 2024-06-24

```c++
// 引入头文件
#include <functional>

 /*  _关键字_   _______类型定义_________    _类型_  */
 /* /       \ /                       \  /     \ */
     typedef  std::function<char*(int)>  MyType
```

可以简化函数指针写法