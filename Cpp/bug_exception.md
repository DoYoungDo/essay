*2025-05-21*

程序在windows上运行时，突然崩溃，崩溃信息如下：

```
底层停止了因为它触发了一个异常。
在线程0中停止，因为: Exception at 0x6e4ed2ca, code: 0xc0000005: read access violation at: 0xffffffffcdcdcdd1, flags=0x0。
```

这种情况大概率就是代码中的某个指针没有初始化，然后直接使用了


记录下出现这种情况的代码：
```cpp
class P* p;

P* getP(){
    // 在p指针没有补初始化时，在windows上使用mavc编译时，p会随机指向一个地址，所以此处判空没有生效
    if(!p)
    {
        p = new P;
    }
    return p
}
```

正常的代码应该是：
```cpp
class P* p = nullptr;
```