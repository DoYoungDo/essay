*2025-06-25*

## 模板函数注意事项

- 模板函数的全部代码（声明和实现）必须写在头文件内，否则编译其他文件时不会生成需要的模板实例化代码。

    ```C++
    // commandmanager.h
    template<typename... Args>
    void registerCommand(const QString& id, const QString& title, std::function<void(Args...)> call);

    // commandmanager.cpp
    template<typename... Args>
    void CommandManager::registerCommand(const QString& id, const QString& title, std::function<void(Args...)> call) {
        // 实现...
    }
    ```

    **这样写会导致链接错误！**