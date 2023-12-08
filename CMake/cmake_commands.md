# CMake常用指令

## `cmake_minimum_required`

> 设置`cmake`最低版本

```
cmake_minimum_required(
    VERSION <最小版本>
)
```

## `project`

> 设置项目名称

```
project(
    <项目名称> 
    [VERSION <版本>]
    [DESCRIPTION <描述>]
    [HOMEPAGE_URL <主页链接>]
    [LANGUAGES <语言>]
)
```

## `message`

> 打印一条日志信息

```
message（
    [<日志模式>] "日志信息" ...
)
```

> 日志模式的取值

```
FATAL_ERROR: 停止cmake进程，停止继续生成

SEND_ERROR: 继续cmake进程，跳过生成

WARNING: 继续cmake进程，打印警告日志

STATUS: ...

DEBUG: ...

```

## `add_executable`

> 通过指定的源文件向项目下添加一个可执行程序

```
add_executable(
    <可执行程序名称>
    [WIN32]
    [MACOSX_BUNDLE]
    [EXCLUDE_FROM_ALL]
    [<源文件1>]
    [<源文件2>]
)
```

## `add_library`

> 通过指定的源文件向项目下添加一个库

```
 add_library(
    <库名称>
    [<库类型>]
    [EXCLUDE_FROM_ALL]
    [<源文件>...])
```

> 库类型的取值

```
STATIC: 静态库
SHARED: 动态库
MODULE
```

> 此指令一般配合下面的`target_link_libraries`命令使用

## `target_link_libraries`

> 为一个库或者标识指定依赖

```
target_link_libraries(
    <目标名称>
    <依赖项>
)
```

> 指定的目标名称必须已经定义
