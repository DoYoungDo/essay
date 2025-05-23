*2025-05-23*

## C++ 一个通过局部变量生命周期控制代码执行的小技巧

```cpp
class LifeCycle{
public:
    void start(){
        cout() << "start exec" << endl;
    }

    void end(){
        cout() << "end exec" << endl;
    }
};

LifeCycle *lifeCycle = new LifeCycle();

void exec(){
    lifeCycle.start();
    ...
    // 如何保证在代码的迭代过程中，下面这行代码一定在此exec函数结束时执行？
    lifeCycle.end();
}
```

**可以通过引进一个局部变量来实现**

```cpp
class FinalRelease{
public:
    FinalRelease(std::function<void(void)> func) : mFunc(func){}
    ~FinalRelease(){
        if(mFunc){
            mFunc();
        }
    }
private:
    std::function<void(void)> mFunc;
}

LifeCycle *lifeCycle = new LifeCycle();

void exec(){
    // 定义一个局部变量，当此函数结束时，局部变量会被释放，而后fr的析构函数被执行，从而执行了lambda函数，从而执行了lifeCycle.end()
    // 这样就保证了lifeCycle.end()一定在exec函数结束时执行
    FinalRelease fr([&](){
        lifeCycle.end();
    });

    lifeCycle.start();
    ...
}
```