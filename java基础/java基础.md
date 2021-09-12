



### war包

war包是一种打包格式。java web工程都是打成war包，进行发布。将war包放在tomcat容器的webapps下，启动服务，即可运行该项目，该war包会自动解压出一个同名的文件夹。

war包中主要是有一个WEB-INF，WEB-INF下面主要有一个lib文件夹，主要存放需要用到的外部依赖的jar包。classes下存放的是带包名结构的java类；classes下还存放有配置文件以及static中的前端资源。

### java获取当前时间 

```java
//使用Calendar
Calendar now = Calendar.getInstance();
System.out.println("年：" + now.get(Calendar.YEAR));
System.out.println("月：" + (now.get(Calendar.MONTH) + 1));
System.out.println("日：" + now.get(Calendar.DAY_OF_MONTH));
System.out.println("时：" + now.get(Calendar.HOUR_OF_DAY));
System.out.println("分：" + now.get(Calendar.MINUTE));
System.out.println("秒：" + now.get(Calendar.SECOND));
```

```java
//使用Date
Date d = new Date();
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
System.out.println("当前时间：" + sdf.format(d));
//sdf.fromat(d)的返回值是String
```

使用System.currentTimeMillis()产生一个从1970年计算到现在的毫秒数，是long类型

### 从List中随机取出一个元素

```java
List<Integer> list = new ArrayList<>();
Random random = new Random();
int n = random.nextInt(list.size());
list.get(n);
```

```java
//获取Set里的元素，将Set转成ArrayList再使用get()方法。
//转成List后，set变得有序，可以获得具体的元素。也可以联合random随机获取其中的元素。
public static void main(String[] args){
    Set<String> set1=new HashSet<>();
    set1.add("1");
    List<String> list=new ArrayList<>(set1);
    String newKey=list.get(0);
    System.out.println(newKey);
}
```

### HashSet

- HashSet基于HashMap来实现的，是一个不允许有重复元素的集合。
- HashSet允许有Null值
- HashSet是无序的，即不会记录插入的顺序
- HashSet不是线程安全的，如果多个线程同时修改HashSet，则最终结果是不确定的。必须在多线程访问时显式同步对HashSet的并发访问。



### java字符串中的==

首先==比较的是字符串的地址。
什么时候会将字符串放入常量池？

```java
public class AA {
	String s0 = new String("aaa");
 
	void fun() {
		String ss = new String("11");
	}
 
	public static void main(String[] args) {
		String s = "A" + "B";
	}
}
```

以上三中赋值String都会放入常量池。
（1）直接引号赋值和（2）new赋值都会放入常量池，（3）而且String s = "A"+"B"，这个语句在编译器优化后常量池会有“AB”，即字符串常量直接相加在编译器就会直接产生相加后的常量。
<font color="red">new赋值会将字符串放入常量池，但是引用仍是堆引用，而非常量池相应引用</font>
**注意只有都是字符串常量相加才会放入常量池**。如果是String s="A"+new String("B")，那么常量池中有A和B，却没有AB。引用的对象相加后是分别存入常量池。
可以理解下面的例子：

```java
	String s1="aa"+"bb";
	String s2="aabb";
	System.out.println(s1==s2);//true
 
	String s1="aa"+new String("bb");
	String s2="aabb";
	System.out.println(s1==s2);//false
	
	String s1 = "aa";
	String s2 = "aabb";
	System.out.println(s1==s1+"bb");//false
```

>String中有一个方法intern：该方法如果常量池中没有则将其放入常量池，如果有则返回常量池相应引用。
><font color="red">当new一个String的对象时，会在常量池和堆中都生成字符串，直接调用intern方法则返回常量池引用。下一行调用则不影响引用。</font>
>**注意intern的失效状态**

```java
//此时创建了2个对象，一个堆内，一个常量池中
        String s1=new String("1");
	String s2="1";
	System.out.println(s1==s2);//false
 
        //s1仍然指向堆中对象，s2指向常量池中字符串
        String s1=new String("1");
        s1.intern();//这句相当于无用，因为常量池已经有了
	String s2="1";
	System.out.println(s1==s2);//false
 
        //intern返回常量池中字符串引用，故相等
	String s1 = new String("1").intern();
	String s2 = "1";
	System.out.println(s1 == s2);//true
 
        //参考下面
	String s1 = new String("1")+new String("2");
	String s2 = "12";
	System.out.println(s1 == s2);//false
 
        //此处jdk1.6及之前为false，jdk1.7开始字符串常量池处于堆内而不是方法区了。
        //以jdk1.7为例，常量池中有1和2，但是没有12。在此情况下调用intern，常量池将存储指向堆内字符串12的引用而非字符串12，即s2其实也是指向堆内12的。
	String s1 = new String("1")+new String("2");
        s1.intern();
	String s2 = "12";
	System.out.println(s1 == s2);//true by jdk1.7+
 
        //虽然也调用了intern，但是s2赋值时常量池没有12，所以将字符串12放入常量池并返回引用。
        //再调用intern相当于无效（常量池已有字符串）
	String s1 = new String("1")+new String("2");
	String s2 = "12";
        s1.intern();
	System.out.println(s1 == s2);//false
```

final也会起到作用：变量与常量的区别。

```java
final String a = "hello";
		String b = "hello";
		String c = a + "a";
		String d = b + "a";
		String e = "helloa";
		System.out.println((d == e));// false
		System.out.println((c == e));// true
```

PS：String s=new String("1")+new String("2");这个语句创建了多少个对象？（个人认为6个，具体请自己javap -v xxx.class查看）

### 单例模式

单例模式属于创建型模式的一种，应用于保证一个类仅有一个实例的场景下，并且提供一个访问它的全局访问的特点。系统从启动到停止，整个过程只会产生一个实例。
**懒汉式：**默认不会实例化，什么时候用什么时候new.初始化的时候才会初始化对象。

```java
public class Lazy{
    //私有化构造器
	private Lazy(){}
    //创造一个私有的实例为static设为null
    private static Lazy lazy = null;
	//默认不会实例化，什么时候用什么时候new
	public static synchronized Lazy getInstance(){
        if(lazy = null){
            lazy=new Lazy();
        }
        return lazy;
	}
}
```

**饿汉式：**类加载的时候实例化，并创建单例对象

```java
pubLic class Hungry{
	private Hungry(){}
	//类加载的时候就实例化,并创建单例对象
	private static Hungry hungry = new Hungry();
	public static Hungry getInstance(){
		return hungry;
	}
}
```

饿汉式线程安全，没有加锁，执行效率高。懒汉式节省内存。







