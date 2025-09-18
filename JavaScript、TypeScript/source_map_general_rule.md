*2024-11-28*

# sourceMap通用规范

>  [source map 可视化验证在线工具](https://evanw.github.io/source-map-visualization/)

#### *示例*

源
```js
// main.js
function greet(name) {
  console.log(`Hello, ${name}!`);
}

greet('World');
```

目标
```js
// main.min.js
function greet(n){console.log("Hello, "+n+"!")}greet("World");
```

映射
```json
// main.min.js.map
{
  "version": 3,
  "file": "main.min.js",
  "sources": ["main.js"],
  "names": ["greet", "name", "console", "log"],
  "sourcesContent":"function greet(name) {\n  console.log(`Hello, ${name}!`);\n}\n\ngreet('World');",
  "mappings": "AAAA,SAASA,CAAT,CAAA,CAAA,CAAA,CAAA,CAAC;AACH,OAAO,CAAC,GAAR,CAAY,OAAZ,CAAA,CAAA,CAAA,CAAA,CAAA,CAAA,CAAA,CAAA,CAAA,CAAA;AAIA,CAAC,CAAD,CAAH,CAAC,CAAD,CAAA,CAAA,CAAA,CAAA,CAAA,CAAA,CAAA"
}

```

#### *说明*

sourceMap是一个标准的json文件，其中包含以下属性

* `version: number`

   版本号，当前固定为`3`

* `file: string`

   目标文件的文件名

* `sources: string[]`

   所有源文件的文件路径，可以是绝对路径也可以是相对路径

* `names: string[]`

   源代码中的所有标识符，调试时可以将生成的代码翻译为原名称

* `sourceRoot？: string`

   源文件的根路径

* `sourcesContent？: string`

   源代码

* `mappings: string`

   映射关系编码后的字符串，使用`VLQ(Variable-Length Quantity)`编码方式
   
   **编码前的规范**
   
   映射关系由多个段落(`Line`)组成，  
   每个`Line`为`生成代码的一行`，每个`Line`由多个部分(`Part`)组成  
   每个`Part`为`生成代码一行中的一段范围`，每个`Part`由4到5个数字组成，分别为：  
   `generatedColumn（生成代码中的列号）`  
   `sourceIndex（源代码文件在‘sources’数组中的索引）`  
   `originalLine（源代码中的行号）`  
   `originalColumn（源代码中的列号）`  
   `nameIndex（名称在‘names’数组中的的索引，可选）`
   
   *上述数字都从`0`开始*
   
   **示例**
   
   *编码前*
   ```json
   // 整个目标文件
   [
      // 一行
      [
         // 一部分
         [0,0,0,0],
         // 另一部分
         [9,0,2,12]
      ],
      // 另一行
      [
         [0,0,2,0],
         [5,0,2,9]
      ]
   ]
   ```
   
   *编码后*
   ```json
   AAAA,SAEY;AAAZ,KAAS
   ```