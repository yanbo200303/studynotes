# 单例模式和Fluent Interface在SDK开发中的应用
在SDK开发的过程中，应用最广泛的开发模式就是单例模式，本文就单例模式的一些应用进行探讨，顺便讨论下Fluent Interface的接口设计风格。
## 懒汉写法
一般的写法如下所示：
```java
public class Singleton {  
	private static Singleton sInstance;  
	private Singleton (){}  

    public synchronized static Singleton getInstance() {  
		if (sInstance == null) {  
			sInstance = new Singleton();  
        }  
		return sInstance;  
   }  
}  
```
第一次调用getInstance时初始化Singleton，但是由于使用synchronized修饰getInstance，因此每次调用都会进行同步操作，这时效率就会很低。
## 饿汉写法
```java
public class Singleton {  
	private static Singleton sInstance = new Singleton();  
	private Singleton (){}  
	public static Singleton getInstance() {  
		return sInstance;  
	}  
}  
```	
使用这种方式，sInstance在类装载时就实例化，避免了每次调用都同步，有效提高了调用时的效率，但是该方式在类装载时就已经加载了，对应用启动的效率就有所影响。如果该类是调用频率不高的类，那么就更加不划算了。<p>
另外，该种写法不支持传入参数，这在很多情况下是不适用的。
## 双重校验锁
```java
public class Singleton {  
    private volatile static Singleton sInstance;  
    private Singleton (){}  
    public static Singleton getInstance() {  
		if (sInstance == null) {  
			synchronized (Singleton.class) {  
				if (sInstance == null) {  
					sInstance = new Singleton();  
				}  
			}  
		}  
		return sInstance;  
    }  
} 
```	
双重校验锁的写法是目前最通用的做法，这种写法既能在需要的时候进行初始化，又能保证线程安全。<p>
但是这种写法一定要注意在sInstance前面加上**volatile**标识符，在很多书上或者网上的教程中，忘记了加上**volatile**，就会导致在使用时，碰到多线程不同步的错误。使用volatile，是利用了volatile的多线程可见性，以及另一个很重要的特性：禁止指令重排序化。
## 枚举Enum
```java
public enum Singleton {  
	INSTANCE;  
	public void whateverMethod() {  
	}  
} 
```	
<p>
这种方式非常简单，而且也是Effective Java作者Josh Bloch 提倡的方式，它不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象。但是很少看到有人这么写，大概是不熟悉的缘故。<p>
## Fluent Interface
使用单例模式的同时，顺便附送一种接口设计：** Fluent Interface **。这种设计在很多开源的类库中都有使用，比如picasso等。
<p>
SDK对外会暴露很多方法，随着SDK的不断演进，原有方法的参数可能就不能满足新的需要，这时一个简单的做法就是重载。重载虽然能够简单的解决问题，但是需求不断改变，需要不断的添加新的参数，不断的增加重载函数，对于SDK开发者和使用者都是一个噩梦。这个时候使用Fluent Interface就非常简洁方便，而且天然向前兼容。<p>
比如，我们有一个函数` exec(int x) `，若我们需要新增一个参数，则我们一般会写成` exec(int x,int y) `，
若我们采用Fluent Interface写法，则我们不需要在exec中指定参数，而直接set到对象中，类实现如下：
```java
public class Fluent {  
	private int x;
	private int y;
	private static Fluent sInstance = new Fluent();  
	private Fluent (){}  
	public static Fluent getInstance() {  
		return sInstance;  
	}    

	public void exec(){...}
	
	public Fluent setX(int x){
		this.x = x;
		return this;
	}
	
	public Fluent setY(int y){
		this.y = y;
		return this;
	}
}  
```	
<p> 调用方法如下：
```java
Fluent.getInstance().setX(1).setY(1).exec();
```
## 小结
一般情况下，若没有参数需要传入，使用饿汉模式是最简单有效的方法；但是若有参数传递，双重检验锁是最好的选择。
