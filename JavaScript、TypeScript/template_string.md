# js模板字符串会隐形调用对象的toString方法

遇到一个问题，在查看堆栈日志时发现这样一个代码
```ts
function assertEqual(a, b, msg, msg2, stackCrawlMark) {
  if (a !== b) {
    const message = msg ? msg2 ? `${msg} ${msg2}` : msg : "";
    fail(`Expected ${a} === ${b}. ${message}`, stackCrawlMark || assertEqual);
  }
}
```

日志中的参数显示是
```
Expected /xxx/xxx/xxx.json === /xxx/xxx/xxx.json.
```

明明从参数上看，两个参数相同，但为什么确能命中上述的断言？

再看下面的代码就一目了然了

```ts
let a = "Hello World";

let b = {
	toString() {
		return "Hello World"
	}
}

console.log(`equeals:${a} === ${b}`,a === b)
// equeals:Hello World === Hello World, false
```

**因为模板字符串会隐形调用非string类型的toString方法**
