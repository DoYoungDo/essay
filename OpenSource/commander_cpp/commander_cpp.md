## 《commander_cpp》单头文件的、链式调用的、自动生成帮助文档的C++命令行参数解析库

### 前言

作者本人经常会写一些命令行小工具，此前一直使用node.js的三方库[commander.js](https://github.com/tj/commander.js)来解析命令行参数,commander.js是一个非常方便的命令行参数解析库，  
但是使用node.js也会有一些限制，比如：运行的电脑上需要安装node.js，并且在一些对性能要求比较高的场景就会不太方便了，  
针对上述情况，我尝试切换到c++，但是在实际写代码时发现，现有的命令行解析库使用时没有commander.js那么丝滑，让我很难受，  
于是，我决定仿照commander.js的设计，写一个c++的命令行参数解析库，取名为commander_cpp。


