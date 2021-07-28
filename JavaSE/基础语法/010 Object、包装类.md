# Object

Object 存在于 java.lang 包，是 java 中**所有类**的**直接**父类或**间接**父类，如果定义一个类没有继承任何类，系统会自动继承 Object 类	



**Object 类中有 12 个方法：**

## 1. registerNatives

```java
private static native void registerNatives();
```

作用：调用该方法完成对该类中本地方法的**注册**

为什么要注册？

​    一个Java程序要想调用一个本地方法，需要执行两个步骤：

1. 通过System.loadLibrary()将包含本地方法实现的动态文件加载进内存；

2. 当Java程序需要调用本地方法时，虚拟机在加载的动态文件中定位并链接该本地方法，从而得以执行本地法

registerNatives()方法的作用就是取代第二步，让程序主动将本地方法链接到调用方（提前定位链接好），当Java程序需要调用本地方法时就可以**直接调用**，而不需要虚拟机再去定位并链接。



## 2. getClass

```java
public final native Class<?> getClass();
```

作用：返回此 Object 的运行时类



## 3. hashCode

```java
public native int hashCode();
```

作用：返回此对象的哈希码值（散列值），该值由对象的属性、内存地址等确定



## 4. equals

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

作用：判断对象是否相等

 == 与 equals 的区别？

Object 中 equals 方法的本质就是利用 == 进行判断，**String 中**对 equals 方法进行了重写，先用 == 判断，再判断内容是否相等，满足其中一个就为 ture

==：判断**栈内存**中的值

equals：内容是否相等（String 重写之后）



## 5. clone

```java
protected native Object clone() throws CloneNotSupportedException;
```

作用：返回当前对象的一个副本，但执行该方法的对象所属的类需要实现 **Cloneable** 接口，否则无法调用，并抛出CloneNotSupportedException 异常

clone 克隆（复制），包括**浅克隆**和**深克隆**，如果一个类的属性只有基本类型，那深克隆和浅克隆是一样的，如果有引用类型，则：

- 浅克隆：拷贝的是引用（**系统默认**）
- 深克隆：新开辟内存空间，进行值拷贝（**需要手动实现**）

```java
package com.dzx.moon;

public class Student implements Cloneable {
    public int age;
    public Student stu;

    public Student(int i) {
        super();
        this.age = i;
    }

    public Student(int age, Student student) {
        super();
        this.age = age;
        this.stu = student;
    }

    @Override
    public String toString() {
        return "[age = " + age + ",stu.age = " + stu.age + "]";
    }

    /**
     * 浅克隆
     * 默认调用父类的clone（），也就是Object中的，即本地方法
     */
    @Override
    protected Object clone() throws CloneNotSupportedException {
        // TODO Auto-generated method stub
        return super.clone();
    }
    
    /**
     * 深克隆
     * 需要自己写，实现新开辟内存空间，进行值拷贝
     */
    @Override
    protected Object clone() throws CloneNotSupportedException {
        Student clone =(Student) super.clone();
        //重写开辟新空间
        clone.stu = new Student(age);
        return clone;
    }
}

//浅克隆
public static void main(String[] args) throws CloneNotSupportedException {
        Student c = new Student(18);
        Student c1 = new Student(18, c);
        Student c2 = (Student) c1.clone();

        System.out.println(c1 == c2);// false
        System.out.println(c1.toString());// [age = 18,stu.age = 18]
        System.out.println(c2.toString());// [age = 18,stu.age = 18]

        c.age = 20;
    	// 引用进行了拷贝，所以指向同一个对象
        System.out.println(c1.toString());// [age = 18,stu.age = 20]
        System.out.println(c2.toString());// [age = 18,stu.age = 20]
        System.out.println(c1.stu == c2.stu);//true
    }

//深克隆
public static void main(String[] args) throws CloneNotSupportedException {
    
		Student c = new Student(18);
        Student c1 = new Student(18, c);
        Student c2 = (Student) c1.clone();

        System.out.println(c1 == c2);// false
        System.out.println(c1.toString());// [age = 18,stu.age = 18]
        System.out.println(c2.toString());// [age = 18,stu.age = 18]

        c.age = 20;
    	// 开辟了新空间，并进行了值拷贝，引用没有拷贝，指向不同的对象
        System.out.println(c1.toString());// [age = 18,stu.age = 20]
        System.out.println(c2.toString());// [age = 18,stu.age = 18]
        System.out.println(c1.stu == c2.stu);//false
	}
```



## 6. toString

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

作用：返回对象的字符串表达形式，默认是 **全类名+@+十六进制的hashCode值**，输出对象时会自动调用



## 7. notify、notifyAll、wait（3种）

在实际的多线程编程中，**只有同步锁对象调这三个方法，才能完成对多线程间的线程通信**	（使线程成为对象监视器的所有者）

通俗来说，以上方法**需要获得锁**，所以一般只能在**同步方法**或者**同步代码块**中调用

```java
public final native void notify();

public final native void notifyAll();

public final void wait() throws InterruptedException {
        wait(0);
    }

public final native void wait(long timeout) throws InterruptedException;

public final void wait(long timeout, int nanos) throws InterruptedException {
        if (timeout < 0) {
            throw new IllegalArgumentException("timeout value is negative");
        }

        if (nanos < 0 || nanos > 999999) {
            throw new IllegalArgumentException(
                                "nanosecond timeout value out of range");
        }

        if (nanos > 0) {
            timeout++;
        }

        wait(timeout);
    }
```

wait()：导致当前线程等待并使其进入到等待阻塞状态。直到其他线程调用该同步锁对象的notify()或notifyAll()方法来唤醒此线程

notify()：唤醒在此同步锁对象上等待的单个线程，如果有多个线程都在此同步锁对象上等待，则会任意选择其中某个线程进行唤醒操作，只有当前线程放弃对同步锁对象的锁定，才可能执行被唤醒的线程。

notifyAll()：唤醒在此同步锁对象上等待的所有线程，只有当前线程放弃对同步锁对象的锁定，才可能执行被唤醒的线程。

**注意：**

- wait()方法执行后，当前线程会释放当前的锁，然后让出CPU，进入阻塞状态
- 只有当 notify/notifyAll() 被执行时候，才会唤醒一个或多个正处于等待状态的线程，然后继续往下执行，直到**执行完synchronized 代码块**的代码或是中途**遇到wait()** ，再释放锁，也就是说，notify/notifyAll() 的执行只是唤醒沉睡的线程，**而不会立即释放锁**
- notify()/notifyAll()执行后，如果后面有sleep()方法，则会使当前线程进入到阻塞状态，但是同步对象锁没有释放，依然自己保留，那么最终还是继续执行此线程
- wait() 需要被try catch包围，以便发生异常中断也可以使wait等待的线程唤醒
- notify 和wait 的顺序不能错，如果A线程先执行notify方法，B线程在执行wait方法，那么B线程是无法被唤醒的
- 当 wait 线程唤醒后并执行时，是接着上次执行到的wait()方法代码后面继续往下执行的



**notify 和 notifyAll的区别**

notify方法只唤醒**一个**等待（对象的）线程并使该线程开始执行。所以如果有多个线程等待一个对象，这个方法只会唤醒其中一个线程，选择哪个线程取决于操作系统对多线程管理的实现。notifyAll 会唤醒**所有**等待(对象的)线程，哪一个线程将会第一个处理取决于操作系统的实现



**wait（long timeout）**

- timeout - 要等待的最长时间（以毫秒为单位）

- 如果 timeout 为零，则不考虑实际时间，在获得通知前该线程将一直等待，**wait（）本质就是wait（0）** 

- wait(long)在无锁竞争情况下，在等待时间过去后就直接重新获取锁，往后执行；但是在竞争条件下，都会等获取到锁了才可以往下执行

- wait(long timeout, int nanos) 也一样



## 8. finalize

```java
protected void finalize() throws Throwable { }
```

作用：protected修饰，子类可以覆盖该方法以实现资源清理工作，GC在回收对象之前调用该方法

- finalize的调用具有**不确定性**

- 不建议用finalize方法完成“非内存资源”的清理工作

- 建议用于：

  ① 清理本地对象(通过JNI创建的对象)；

  ② 作为确保某些非内存资源(如Socket、文件等)释放的一个补充



Java 技术允许使用 finalize() 方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。这个方法是由垃圾收集器在确定这个对象**没有被引用时**对这个对象调用的。它是在 Object 类中定义的，因此所有的类都继承了它。子类覆盖 finalize() 方法以整理系统资源或者执行其他清理工作。finalize() 方法是在垃圾收集器删除对象之前对这个对象调用的。注意：finalize不一定被jvm调用，只有当垃圾回收器要清除垃圾时才被调用

![1624278802511](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1624278802511.png)

finalize 大致流程：当对象变成(GC Roots)不可达时，GC会判断该对象是否覆盖了finalize方法，若未覆盖，则直接将其回收。否则，若对象未执行过finalize方法，将其放入F-Queue队列，由一低优先级线程执行该队列中对象的finalize方法。执行finalize方法完毕后，GC会再次判断该对象是否可达，若不可达，则进行回收，否则，对象“复活”。

finalize 方法**一个对象只能执行一次**，只能在第一次进入被回收的队列，而且对象所属于的类重写了finalize方法才会被执行。第二次进入回收队列的时候，不会再执行其finalize方法，而是直接被二次标记，在下一次GC的时候被GC



Object 中经常被重写的方法

1、public String toString()：以字符串的形式返回对象的信息 

2、public boolean equals(Object obj)：判断两个对象是否相等 

3、public native int hashCode()：返回对象的散列码





# 包装类

java 提供的一组类，专门用来**创建**8种基本数据类型对应的**对象**，共有8个包装类

**Byte**				byte

**Short**				short

**Integer**				int

**Long**				long

**Float**				float

**Double**				double

**Character**			char

**Boolean**				boolean



包装类的体系结构：

![1614769604621](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1614769604621.png)



## 装箱和拆箱

- 装箱（将基本数据类型转化为包装类对象）

**创建对应的实例化对象**

每个包装类都提供了相应的构造函数（Type/String/char）

```
type表示8种基本数据类型，Type表示8种封装类
1.public Type(type value)

2.public Type(String value)[非Character类]/public Type(char value)[Character类]
！！！注意 Boolean 构造函数当参数不为 "true"时，值为 False
```

常用方法：

public static **valueOf**(type value/String value/char value) ：返回对应的包装类（每个包装类都有）



- 拆箱（将包装类对象转化为基本数据类型）

常用方法： 

1. type*Value（）方法，将包装类转为基本数据类型

```java
Byte byt = Byte.valueOf("1");
byte b = byt.byteValue();

Double dou = Double.valuOf("1");
double d=dou.doubleValue();
```

2. **parse*type**（String value）：**静态方法**，将 String 类型转为基本数据类型（Character类除外） 

```
byte b = Byte.parseByte("1");
```

3. Type.toString(type value) ：将基本数据类型转为 String 类型

```java
byte b = 1;
String str = Byte.toString(b);
```







