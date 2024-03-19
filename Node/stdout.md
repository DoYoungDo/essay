# node js的标准输出

node js中的标准输出不是`consolo.log()`而是`process.stdout.write(msg)`

一直在一行打印日志是，原理是将光标移到开头
```js
process.stdout.write(msg + '\r')
```

清空一行日志
process.stdout.clearLine();
