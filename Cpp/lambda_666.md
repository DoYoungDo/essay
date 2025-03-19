## C++ lambda表达式的骚操作

这是正常操作

```cpp
struct Person{
    string name;
    int age;
}

Person p = {
    "zhang san",
    18
}

auto doSometing = [p]()->void{
    cout << "name is: << p.name;
}

```

这是骚操作

```cpp
struct Person{
    string name;
    int age;
}

Person p = {
    "zhang san",
    18
}

// 直接在捕获的位置定义一个临时变量
auto doSometing = [name = p.name]()->void{
    cout << "name is: << name;
}
```

不过不推荐这么写，虽然能编译通过，但是一般的编辑器会报红