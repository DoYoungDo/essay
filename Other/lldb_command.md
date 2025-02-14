# lldb调试常用命令

> 2024-08-20

*启动lldb*

```shell
lldb
```

*查看帮助*

```shell
help
```

*挂载进程*

```shell
lldb

attach --pid 进程id

attach --name 进程名
```

*显示指定的源文件*

```shell
lldb

l main.cpp
```

*查看所有断点*

```shell
lldb
b 
```

*添加断点*

```shell
lldb

b 行号
```

*打印值*

```shell
lldb

p
```

*继续执行*
```shell
lldb
c
```

*单步执行*
```shell
lldb
n
```

*单步进入*
```shell
lldb
s
```

*单步跳出*
```shell
lldb
finish
```