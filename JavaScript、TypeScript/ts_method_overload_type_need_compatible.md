# ts函数重载参数类型需要能兼容

> 函数重载时按定义的顺序检查类型
>
> 下面的代码会报错
> 
> This overload signature is not compatible with its implementation signature.

```typescript
function abc():void;
function abc(a:string):void{
    console.log(a)
}
```

> 上面的代码，ts会用带参数的声明去兼容没有参数的声明，必传参数无法兼容没有参数的类型
> 
> 改成下面代码

```typescript
function abc(a:string):void;
function abc():void{
    // console.log(a)
}
```
或
```typescript
function abc():void;
function abc(a?:string):void{
    console.log(a)
}
```