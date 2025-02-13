# lldb调试常用命令

> 2024-08-20

*启动lldb*

```shell
lldb
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