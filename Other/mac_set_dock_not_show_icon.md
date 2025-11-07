*2025-11-07*

## mac设置程序坞不显示图标

修改info.plist,添加条目：

```xml
    <key>LSUIElement</key>
    <true/>
```