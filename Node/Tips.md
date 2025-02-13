# node小技巧

> 2024-11-01

## node在执行js文件时，可以忽略.js后缀

```shell
$ node index.js
    ⬇⬇⬇
$ node index
```

## node在执行js文件时，可以可以直接执行一个目录,如果这个目录中有index.js文件

目录结构如下

```
└─dist/
  └─index.js
```

<span style="color:lightgreen">⬇⬇︎⬇︎︎︎︎︎︎︎</span>

可以直接如下执行

```shell
$ node dist 
```

---

# node小技巧

## 不可以对一个右值进行++或--

```ts
// 以下都是错误
++1
1++
--1
1--

let _map:Map<string,number> = new Map;
_map.set("one",1);

_map.set("tow",_map.get("one")++) // 错误

// 正确写法
let a = 1;
++a
a++
--a
a--

let _map:Map<string,number> = new Map;
_map.set("one",1);
let v = _map.get("one")
_map.set("tow",v++) // 正确


```

---

# node小技巧

## node fork子进程不执行

> 2024-12-17

node fork子进程时需要注意，当前进程是否是调试启动的  
如果是调试启动的，被fork的子进程默认也会是调试启动的，  
**注意**，**这时，被fork出来的子进程会默认在第一行打个断点，就会一直卡在第一行断点<span style="color:red">不执行</span>**  
解决办法有两种：
1. attach到这个子进程，将这个断点放掉
2. fork子进程时，不将当前进程的默认参数传入到子进程中，示例代码:

    ```ts
    child_process.fork("xxx/xxx/xxx.js", ["-a","-b"],{
        // 将此数组传空
        execArgv: []
    }
    ```
