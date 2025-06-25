*2025-06-25*

## js、ts中的标签使用

js中没有goto语句，但是有标签语法，标签语法多用于跳出多重循环。

### 定义和跳转标签

使用标签必须搭配break或continue语句

```js
label:
break label;
continue label;
```

### 示例

*跳出多重循环*

```js
let i = 0;
outer: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        if (i === 1 && j === 1) {
            break outer;
        }
    }
}
```

*跳出代码块*

```js
let i = 0;
outer: {
    console.log(i);
    break outer;
    console.log('不会执行');
}
console.log('循环结束');
// 0
// 循环结束
```

*跳出循环或代码块嵌套switch语句*

```js
let i = 0;
outer: {
    switch (i) {
        case 0:
            console.log('i等于0');
            break;
        case 1:
            console.log('i等于1');
            break;
        default:
            console.log('i不等于0或1');
            break;
    }
    console.log('不会执行');
}
console.log('循环结束');
// 0
// 不会执行
// 循环结束
```
