# ts异常结果不能指定具体类型

> 下面的代码没有问题

```typescript
try{
    doSomething();
}
catch(e){
    console.log(e);
}
```

> 下面的代码会报错
> 
> Catch clause variable type annotation must be 'any' or 'unknown' if specified.

```typescript
type MyError = {
    code: number
    msg: string
}

try {
    throw {
        code: 123,
        msg: "error"
    } as MyError
}
catch(e:MyError){
    
}
```

> ts 不能显示指定异常的结果的类型，因为抛出的异常可能是任意类型，需要用下而后方式使用自定义类型

```typescript
type MyError = {
    code: number
    msg: string
}

try {
    throw {
        code: 123,
        msg: "MyError"
    } as MyError;
    
    throw new Error("Error")
}
catch(e){
    if(e instanceof MyError){
        (e as MyError).code
        (e as MyError).msg
        // ...
    }
}
```