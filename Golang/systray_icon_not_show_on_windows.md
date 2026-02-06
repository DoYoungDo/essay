*2026-02-06*

## Go 使用`github.com/energye/systray`设置Windows平台的图标不显示的问题

使用`github.com/energye/systray`设置托盘图标时，需要一个[]byte类型的图标数据,或者使用 `embed` 嵌入图标文件。

通常可以使用 `xxd -i xx.png > xx.go` 或 `2goarray $VAR_NAME $PACKAGE_NAME < xx.png > xx.go` 命令将图标文件转换为Go语言的字节数组。

但是在Windows平台必须使用`.ico`格式的图标文件,否则图标不会显示, mac平台都可以显示
