*2025-02-28*

## Qt避坑-QTableWidget接收不到鼠标移动事件

在监听`QTableWidget`的鼠标移动事件时，会发现在一些时候，获取不到鼠标移动事件。

原因可能是有以下几点：
1. 没有设置`QTableWidget`的`mouseTracking`属性为`true`。
2. 没有设置`QTableWidget`的`viewport`的`mouseTracking`属性为`true`。
3. 设置了cellwidget。

比较坑的是第3点，设置了cellwidget后，顶层控件是cellwidget,  
cellwidget会优先拿到鼠标移动事件，  
而这时，如果cellwidget对鼠标移动事件感兴趣，那么鼠标移动事件就会被cellwidget消费掉，而不会传给`QTableWidget`。

解决办法是，在cellwidget的鼠标移动事件中，调用`event->ignore()`，忽略掉鼠标移动事件。

以QLabel为例：
QLablell对鼠标移动事件默认是accept的, 所以需要在鼠标移动事件中调用`event->ignore()`，忽略掉鼠标移动事件。  
而光设置ignore是不够的，还需要设置mouseTracking为true。  
QLabel的mouseTracking默认是不固定的，需要手动统一设置
