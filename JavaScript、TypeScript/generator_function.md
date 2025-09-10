*2025-08-11*

## js/ts中的生成器函数、yiled的使用

生成器函数可以先执行一部分代码后临时暂停执行，后续可以继续执行

配合内存优化食用更佳，使用生成器函数可以不用一次性将数据都加载到内存中

### 定义生成器函数

使用`function*`关键字定义生成器函数，生成器函数的返回值是一个生成器对象。

```js
function* generatorFunction() {
  yield 1;
  yield 2;
  yield 3;
}
```

*注意1：箭头函数不能用来定义生成器函数。*

*注意2：`function*`和`*`是两个单独的标记，因此它们可以用空白或换行符分隔。*

### `yield`关键字

`yield`关键字可以暂停生成器函数的执行，并返回一个值。当调用生成器对象的`next()`方法时，生成器函数会从上次暂停的位置继续执行。

### 生成器对象

生成器对象是一个特殊的对象，它有一个`next()`方法。调用`next()`方法会执行生成器函数的代码，直到遇到`yield`关键字或函数结束。

### `next`方法

`next`方法返回一个对象，对象有两个属性：`value`和`done`。`value`属性是`yield`关键字返回的值，`done`属性是一个布尔值，用于表示生成器函数是否执行完毕。

### 示例

```js
1    function* myGenerator() {
2      yield 1;        // 第一次 next() 会停在这里
3      yield 2;        // 第二次 next() 会停在这里
4      return 3;       // 第三次 next() 会结束生成器
5    }
6    
7    const gen = myGenerator(); // 直接调用生成器函数，返回一个生成器对象,生成器函数体中的代码不会执行，直到第一次调用next方法
8    
9    console.log(gen.next()); // { value: 1, done: false }
10   console.log(gen.next()); // { value: 2, done: false }
11   console.log(gen.next()); // { value: 3, done: true }
12   console.log(gen.next()); // { value: undefined, done: true }
```

代码执行顺序：(行号)

`7` -> `9` -> `2` -> `10` -> `3` -> `11` -> `4` -> `12` -> 结束