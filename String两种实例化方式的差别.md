我们知道String有两种实例化方式：

```java
1. String str1 = "hello";
2. String str2 = new String("hello");
```

使用过程中，都说要使用第一种，第一种好，但是你知道为什么吗？今天我们就讲一下两种String实例化的差别。

###str1的方式
![屏幕快照 2016-02-13 12.04.05.png](https://ooo.0o0.ooo/2016/02/12/56beb6d95822d.png)

###str2的方式
![屏幕快照 2016-02-13 12.11.30.png](https://ooo.0o0.ooo/2016/02/12/56beb712261b0.png)

可以发现，str2开辟了两块内存空间。

###总结
str1的实例化方式相比于str2，除了简单方便之外，还可以节省内存开销，所以推荐使用第一种实例化String。