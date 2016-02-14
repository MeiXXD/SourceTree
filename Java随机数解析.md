我们知道，Java中API的随机数有两种方式，一个是Math的Random方法，一个是Util的Random()方法。

###用法的差别
1. Math生成随机数

```java
//返回一个0.0到1.0的double值
Math.random();
```

2. Util生成随机数

```java
//没有预设种子
Random random = new Random();
//获取随机数，以int为例
random.nextInt();
//返回0到100的随机数
random.netInt(100);

//预设种子
Random random1 = new Random(100);
//或者
Random random2 = new Random();
random2.setSeed(100);
```

###Util随机数详解
先来看段代码：

```java
import java.util.Random;

public class Test02 {
	public static void main(String[] args) {
		Random random1 = new Random();
		Random random2 = new Random();
		for (int i = 0; i < 10; i++) {
			System.out.print(random1.nextInt(100) + ",");
			System.out.print(random2.nextInt(100) + " ");
		}
	}
}
```

运行结果为：15,92 2,0 7,91 34,69 93,32 82,56 62,77 60,60 81,17 53,88 
**结论：random1和random2，都没有预设种子，他们生成的随机数并不同。**

再看下面这段代码：

```java
import java.util.Random;

public class Test02 {
	public static void main(String[] args) {
	  //设置种子都为100
		Random random1 = new Random(100);
		Random random2 = new Random(100);
		for (int i = 0; i < 10; i++) {
			System.out.print(random1.nextInt(100) + ",");
			System.out.print(random2.nextInt(100) + " ");
		}
	}
}
```

运行结果为：15,15 50,50 74,74 88,88 91,91 66,66 36,36 88,88 23,23 13,13 
**结论：相同的种子，生成相同的随机数序列。**

这是为什么呢？看来有必要详细看一下random()函数。
查看Random()函数的定义：

```java
public Random() {
  //这里的this就是包含种子参数的Random函数
  this(seedUniquifier() ^ System.nanoTime());
}
```

注：System.nanoTime()是纳秒形式的JVM当前时间

可以发现：
>Random()函数其实最终还是调用包含seed参数的Random(long seed)，只不过他的种子是JVM当前的纳秒时间经过异或运算后的结果。因为每次的时间都不一样，所以种子不同，导致每次产生的都是不一样的随机数。如果种子相同，则会产生相同的随机数序列。所以，计算机产生随机数，其实是一种**伪随机数**。

>**这里纠正一下大多人笔记中，说这里的种子是JVM当前毫秒时间（System.currentTimeMillis()）。**

###Math随机数详解
查看Math中random()的定义：

```java
public static double random() {
  return RandomNumberGeneratorHolder.randomNumberGenerator.nextDouble();
}
```

再查看randomNumberGenerator的定义：

```java
private static final class RandomNumberGeneratorHolder {
  static final Random randomNumberGenerator = new Random();
}
```

我们发现：
>Math产生随机数的方式还是通过调用Util中不包含预设参数的Random()函数。

###两种生成随机数方式的比较
1. Math生成随机数，很显然，类直接调用，不需要生成具体的对象，书写起来比较方便。缺点是：操作性不足。
2. Util生成随机数，需要创建具体对象。因为可以手动设置种子，所以想要随机序列循环出现，则操作很容易，只要给相同的种子即可。缺点：只要得到了种子，即可复现整个随机序列。