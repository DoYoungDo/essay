# AppleScript

## 简介

……

## 示例

### 打开Safari浏览器
```shell
$ osascript -e 'tell app "Safari" to activate'
```

### 关闭Safari浏览器
```shell
$ osascript -e 'quit app "safari.app"'
or
$ osascript -e ‘tell application “safari” to quit’
```

### 清空回收站
```shell
$ osascript -e 'tell application "Finder" to empty trash'
```

### 设置系统主题
```shell
$ osascript -e 'tell application "System Events" to tell appearance preferences to set dark mode to not dark mode'
```

### 不带询问提示的关机
```shell
$ osascript -e 'tell app "System Events" to shut down'
```

### 不带询问的重启
```shell
$ osascript -e 'tell app "System Events" to restart'
```

### 使用访达打开下载目录
```shell
$ osascript -e 'tell application "Finder" to open (path to downloads folder as text)'
```

### 使用访达打开桌面目录
```shell
$ osascript -e 'tell application "Finder" to open (path to desktop folder as text)'
```

### 使用访达打开文稿目录
```shell
$ osascript -e 'tell application "Finder" to open (path to documents folder as text)'
```

### 模拟按键ctrl + ->
```sh
$ osascript -e 'tell application "System Events" to key code 124 using control down'
```

## 参考
[applescript官方文档](https://developer.apple.com/library/archive/documentation/AppleScript/Conceptual/AppleScriptLangGuide/introduction/ASLR_intro.html#//apple_ref/doc/uid/TP40000983-CH208-SW1)

[applescript入门指南](https://macosxautomation.com/applescript/firsttutorial/index.html)

[osascript man文档](https://ss64.com/mac/osascript.html)

[手把手教你用 AppleScript 模拟鼠标键盘操作，实现 macOS 系统的自动化操作](https://sspai.com/post/43758)