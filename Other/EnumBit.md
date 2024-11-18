# 枚举值的位运算应用和原理

### 枚举值需要是2的幂次方

```ts
enum Color{
   Red = 1 << 0,    // 0001
   Green = 1 << 1,  // 0010
   Blue = 1 << 2    // 0100
}
```

### 多枚举随意组合

```ts
// 红绿
let color:Color = Color.Red | Color.Green
```

```ts
// 红蓝
let color:Color = Color.Red | Color.Blue
```

```ts
// 绿蓝
let color:Color = Color.Green | Color.Blue
```

```ts
// 红绿蓝
let color:Color = Color.Red | Color.Green | Color.Blue
```

### 随意校验

```ts
// 校验是否有`红`
let color:Color...
if(!!(color & Color.Red)){
   ...
}

// 校验是否有`绿`
let color:Color...
if(!!(color & Color.Green)){
   ...
}
```

### 枚举的值必须是1、2、4、8、16这种数的原因

十进制：1、2、4、8、16

 ⬇️ ⬇️ ⬇️ ⬇️ ⬇️ 
 
二进制：1、10、100、1000、10000

> 上面的数转换到二进制后，只有一位是1，确保了数字的独立性，不会出现重复项
