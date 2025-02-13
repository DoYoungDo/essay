# npm安装本地项目到全局

> 2024-05-27

`package.json`配置bin

```json
{
	"bin": {
      "todo": "./out/index.js"
   } 
}
```

安装到全局

```bash
npm install . -g
```

执行命令

```bash
todo -V
```