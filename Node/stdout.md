# node js的标准输出

node js中的标准输出不是`consolo.log()`而是`process.stdout.write(msg)`

一真在一行打印日志是
```js
process.stdout.write(msg + '\r')
```