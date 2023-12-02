# ts中的元组

> 巧用ts中的元组可以省好多事

- 函数返回值想返回多种类型的数据又不想定义一个复杂的类型？

```typescript
function call():[string, number]{
    return ["name",18]
}

call()[0];
call()[1];
```

