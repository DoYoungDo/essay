*2025-07-22*

## Python代码中到处可见的`if __name__ == '__main__':`的含义

1. `__name__`是一个内置的变量
2. 每个模块中都有一个此变量
3. 在程序运行时，会被Python解释器赋于不同的值
4. 当前为文件为主运行文件时, `__name__`的值为`__main__`
5. 当前为文件为被导入运行时, `__name__`的值为文件名(不含后缀)

*示例*

```python
# module.py
print(__name__)

# main.py
import module
print(__name__)

```

```bash
python main.py
# 输出：module
# 输出：__main__

pytho module.py
# 输出：__main__

```

所以`if __name__ == '__main__':`的作用是：
1. 当文件为主运行文件时，执行其中的代码
2. 当文件为被导入运行时，不执行其中的代码
3. 可以在文件中编写测试代码，用于调试



