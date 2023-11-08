# ts的d.ts文件中声明的全局类型

- 在 ts 文件中使用`declare`声明了全局类型的同时，不能再使用`import`或`export`，

```typescript
// index.ts
import { fun } from "./abc"
// 不会生效
declare interface MyType{
        name: string;
}

// index.d.ts
export {};
```

- ts会将使用这两个符号的文件做为一个包，

- 生成d.ts时不会将这个类型生成出来，生成的js源码也没有这个全局类型

- 再在其它文件中使用全局类型会找不到

- ts server 也不会提供相关类型的代码提示，就算手动在d.ts中补了这个类型也不会提示

- 如果想在使用了`import`或`export`文件中使用全局类型，需要写到`global`中

```typescript
import { fun } from "./abc"

declare global{
    interface MyType{
        name: string;
    }
}
```