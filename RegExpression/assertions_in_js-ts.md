# js/ts中的正则断言

## 概念

> 零宽：只匹配位置，在匹配过程中，不占用字符，所以被称为零宽
>
> 先行：正则引擎在扫描字符的时候，从左往右扫描，匹配扫描指针未扫描过的字符，先于指针，故称先行
>
> 后行：匹配指针已扫描过的字符，后于指针到达该字符，故称后行，即产生回溯
>
> 正向：即匹配括号中的表达式
>
> 负向：不匹配括号中的表达式

## 零宽正向先行断言，又称正向向前查找（positive lookhead）

```js
(?=pattern) // 后面有 pattern
```

## 零宽负向先行断言，又称负向向前查找（negative lookhead）

```js
(?!pattern) // 后面没有 pattern
```

## 零宽正向后行断言，又称正向向后查找（positive lookbehind）

```js
(?<=pattern) // 前面有 pattern
```

## 零宽负向后行断言，又称负向向后查找（negative lookbehind）

```js
(?<!pattern) // 前面没有 pattern
```

