# node小技巧

## node在执行js文件时，可能忽略.js后缀

```shell
$ node index.js
    ↓
$ node index
```

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