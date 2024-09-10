# QHash/QMap索引访问有坑

**QHash、QMap使用`[]`访问元素，如果没有访问的key，会自动创建一个值插入到内部，使用`contains`判断时，会为true**

```cpp
QHash<QString,QString> hash;
// hash.insert("key","value");
hash.insert("key1","value1");
hash.insert("key2","value2");
hash.insert("key3","value3");

qDebug() << hash["key"]; // true
```