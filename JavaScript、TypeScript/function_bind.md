*2025-08-29*

## js/ts的`Function.prototype.bind()`方法避坑

函数的bind方法返回一个新方法，多次bind返回的方法不是同一个方法

![alt text](./function_bind/image.png)

![alt text](./function_bind/image1.png)

之前写的蠢代码：  
事件监听时监听了：xxx.func.bind(this)  
取消监听时两次使用了bind，获取到一个新方法，取消监听了：xxx.func.bind(this), 从而取消监听失败