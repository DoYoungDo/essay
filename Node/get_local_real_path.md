# node js在大小写不敏感的系统上获取本地真实文件路径

> 2024-06-05

```ts
function toLocalRealPath(filePath: string): string {
    try{
        if(fs.existsSync(filePath)){
            return fs.realpathSync.native(filePath);
        }
    }catch(e){}
    return filePath;
}
```