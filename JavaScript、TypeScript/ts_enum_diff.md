*2025-08-26*

## ts枚举类型不同写法编译为js后的区别

ts枚举类型的值可以是数字和字符串，但是数字和字符串的写法编译为js后却不相同

### 字符串枚举

```ts
enum StringEnum {
    A = 'a',
    B = 'b',
}
enum StringEnum2 {
    ["A"] = 'a',
    ["B"] = 'b',
}
```

编译为js后

```js
var StringEnum;
(function (StringEnum) {
    StringEnum["A"] = "a";
    StringEnum["B"] = "b";
})(StringEnum || (StringEnum = {}));
var StringEnum2;
(function (StringEnum2) {
    StringEnum2["A"] = "a";
    StringEnum2["B"] = "b";
})(StringEnum2 || (StringEnum2 = {}));
```

### 数字值枚举

```ts
enum NumberEnum {
    A = 1,
    B = 2,
}
enum NumberEnum2 {
    ["A"] = 1,
    ["B"] = 2,
}
```

编译为js后

```js
var NumberEnum;
(function (NumberEnum) {
    NumberEnum[NumberEnum["A"] = 1] = "A";
    NumberEnum[NumberEnum["B"] = 2] = "B";
})(NumberEnum || (NumberEnum = {}));
var NumberEnum2;
(function (NumberEnum2) {
    NumberEnum2[NumberEnum2["A"] = 1] = "A";
    NumberEnum2[NumberEnum2["B"] = 2] = "B";
})(NumberEnum2 || (NumberEnum2 = {}));
```

### 混合枚举

```ts
enum MixEnum {
    A = 'a',
    B = 1,
}
```

```js
(function (MixEnum) {
    MixEnum["A"] = "a";
    MixEnum[MixEnum["B"] = 1] = "B";
})(MixEnum || (MixEnum = {}));
```

### 结论1

- 会发现枚举类型编译为js后，会转换为一个对象，枚举的key作为对象的key,枚举的value作为对象的value
- 如果枚举的值为数字，则会先将枚举的key做对象的key,枚举的value做对象的value,然后再将枚举的value做对象的key,枚举的key做对象的value
- 包括混合值枚举类型中的数字值也同上

### 结论2

```ts
console.log(Object.values(StringEnum));
console.log(Object.values(StringEnum2));
console.log(Object.values(NumberEnum));
console.log(Object.values(NumberEnum2));
console.log(Object.values(MixEnum));
```
输出
```shell
[ 'a', 'b' ]
[ 'a', 'b' ]
[ 'A', 'B', 1, 2 ]
[ 'A', 'B', 1, 2 ]
[ 'B', 'a', 1 ]
```
- 枚举=对象，那么我们就可以对枚举类型进行for循环遍历
- `但是!`由于枚举值为数字类型时的特殊情况，尽量不要遍历枚举值中带数字和枚举，否则容易出现问题