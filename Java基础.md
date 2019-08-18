<<<<<<< HEAD
# 1. 数据类型 #
##
## 基本类型
- byte/8
- char/16
- short/16
- int/32
- float/32
- long/64
- double/64
- boolean/~  
boolean 只有两个值:true false,可以使用 1 bit 来存储,但是具体大小没有明确规定。JVM 会在编译时期将 boolean 类型的数据转换为 int,使用1来表示 true,0表示false。JVM 支持 boolean 数组，但是是通过读写 byte 数组来实现的。
## 包装类型  
基本数据类型都有与其相对应的包装类型。  
包装类型中含有静态内部类(IntegerCache)作为缓存区。  
缓存区会缓存一定范围内的包装类对象(Integer[-128~127])，调用valueOf()方法时，如果值在范围内则 返回缓存区内的对象，如果超出范围则构造新的包装类。 

    public static void main(String[] args) {
    	Integer x = new Integer(1);
    	Integer y = new Integer(1);
    	System.out.println(x == y);
    	//false new Integer会重新创建一个对象
    	Integer z = Integer.valueOf(1);
    	Integer j = Integer.valueOf(1);
    	System.out.println(z == x);
    	//false z是IntegerCache中的new 出来的Integer
    	System.out.println(z == j);
    	//true j也是IntegerCache中的new 出来的Integer
    }
# 2. String #
##
String的不可变有两个方面  
一是String类是final的表示String类不可被继承  
二是String类中含有final 类型的属性。  

    public final class String
	    implements java.io.Serializable, Comparable<String>, CharSequence {
	    /** The value is used for character storage. */
	    private final char value[];
    }

## 不可变的好处 ##
1. 因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。
2. String的不可变保证了在多线程环境下的安全性。任何线程都无法改变String的值所有是线程安全的。
3. 为了实现String Pool
## String，StringBuilder，StringBuffer ##
String,StringBuffer是线程安全的。StringBuffer通过synchronized关键字实现线程安全。
## String Pool ##
String Pool保存着所有编译期确定的字面串变量，除此之外还可以调用str.intern()来将字符串保存到String Pool中。  
当调用str.intern()时，如果String Pool中存在和String相同的值则返回该字符串的引用，如果不存在则将当前字符串加入到String Pool中。

    String s1 = new String("aaa");	//String Pool中没有"aaa"时会创建2个对象，一个在
    String s2 = new String("aaa");	//堆，另一个在String Pool，之后返回堆中对象的引用
    System.out.println(s1 == s2);   // false
    String s3 = s1.intern();		
    String s4 = s1.intern();
    System.out.println(s3 == s4);   // true
	String s5 = "aa";				//会直接将字符串放到String Pool
Java7之前String Pool存放在永久代中。Java7开始将String Pool移动到了堆中。这是为了防止在大量使用字符串的情况下导致永久代出现OutOfMemoryError。
# 3.类加载过程  #
##
类加载的过程遵循：static代码块=static属性>非static代码块=非static属性>构造方法 的顺序。
当调用类的静态属性时不会导致非静态属性的加载。
并且加载外部类时不会导致内部类的加载，只有显式调用内部类时内部类才开始加载。  

    public class ClassLoadingSeq {
    	public static long outterStatic = System.currentTimeMillis();
    	public long outter = System.currentTimeMillis();
    	
    	static {
    		try {
    			Thread.sleep(1);
    		} catch (InterruptedException e) {
    			e.printStackTrace();
    		}
    		System.out.println("static block:"+System.currentTimeMillis());
    	}
    	
    	{
    		try {
    			Thread.sleep(1);
    		} catch (InterruptedException e) {
    			e.printStackTrace();
    		}
    		System.out.println("block:"+System.currentTimeMillis());
    	}
    
    	public ClassLoadingSeq() {
    		try {
    			Thread.sleep(1);
    		} catch (InterruptedException e) {
    			e.printStackTrace();
    		}
    		System.out.println("constructor method :"+System.currentTimeMillis());
    	}
    	
    	public static void main(String[] args) {
    		ClassLoadingSeq outter = new ClassLoadingSeq();
    		System.out.println("outter:"+outter.outter);
    		System.out.println("outterStatic:"+ClassLoadingSeq.outterStatic);
    		//static block:1565151660016
    		//block:1565151660018
    		//constructor method :1565151660020
    		//outter:1565151660016
    		//outterStatic:1565151660013
    		//注释掉前两句后执行结果如下
    		//static block:1565151752345
    		//outterStatic:1565151752343
    	}
    }
# 4.继承 #
##
## 抽象类和接口 ##
- 概念  
抽象类和抽象方法都被 abstract 修饰，如果一个类含有抽象方法那它一定是一个抽象类。  
抽象类和类之间的主要区别在于抽象类不能被实例化，只能通过子类继承才能完成实例化。
接口是一种特殊的抽象类。在Java 8 之前，它可以看成是一个完全抽象的方法，也就是说它不能实现任何类。  
在Java 8 之后，接口也允许实现默认的方法，这是因为不支持默认方法的接口的维护成本太高了。在Java 8 之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类。  
默认方法的使用和抽象类中的非抽象方法一样，可以被实现了接口的子类所调用。 
接口中的所有成员都是 public 的，所有字段都是 static final 的。  

	    public interface Interface {
	    	default void fun() {
	    		System.out.println("fun");
	    	}
	    }
	    public class Children implements Interface {
	    	public static void main(String[] args) {
	    		Children children = new Children();
	    		children.fun();
	    	}
	    }

- 比较  
抽象类和类之间是一种 IS-A 关系，它要求子类和抽象类之间满足里式替换原则。而接口和类之间是一种 LIKE-A 关系，只需要子类和接口有公共之处即可。  
一个类只能继承一个抽象类，但是可以实现多个接口。  
接口中的成员都是 public 的，抽象类的成员可以有多种访问权限。  
接口中的字段都是 static final ，抽象类和类一样。
## 重写与重载
重写指子类实现了一个与父类在方法声明上完全相同的一个方法。  
为了满足里氏替换原则，子类的方法返回值要小于父类，抛出的异常要小于父类，访问修饰限定符要大于父类。  
重载指：同一个类中实现了多个同名的方法。这些方法的参数类型、个数、顺序不同。重载和返回值的类型无关。
## 5.Object通用方法 ##
### 概览 ###
    public native int hashCode()
    
    public boolean equals(Object obj)
    
    protected native Object clone() throws CloneNotSupportedException
    
    public String toString()
    
    public final native Class<?> getClass()
    
    protected void finalize() throws Throwable {}
    
    public final native void notify()
    
    public final native void notifyAll()
    
    public final native void wait(long timeout) throws InterruptedException
    
    public final void wait(long timeout, int nanos) throws InterruptedException
    
    public final void wait() throws InterruptedException
### 重写equals时要注意哪些事情  ###
- 自反性	`x.equals(x);//true`
- 对称性	`x.equals(y)==y.equals(x);`
- 传递性	`if(x.equals(y)&&y.equals(z)) x.equals(z);//true`
- 一致性:多次调用 equals 返回的结果不变。
- 与null值比较 `x.equals(null);\\false`
- equals()比较相等的两个数它的 hashCode() 一定相等。而 hashCode() 相等的数不equals()比较不一定相等。Java中用到hash相关的集合时，我们可能希望将两个equals比较相等的两个对象当成一样的，只在hash中存储一个对象。而如果没有重写 hashCode() 就会在hash中存2个对象。
### 怎么重写 hashCode() ###
    @Override
	public int hashCode() {
		int result = 0;/设置一个随机的result
		result = result*31 + a;
		return result;
	}
## 关键字 ##
## 反射 ##
## 异常 ##
## 泛型 ##
## 注解 ##
=======
# 1. 数据类型 #
##
## 基本类型
- byte/8
- char/16
- short/16
- int/32
- float/32
- long/64
- double/64
- boolean/~  
boolean 只有两个值:true false,可以使用 1 bit 来存储,但是具体大小没有明确规定。JVM 会在编译时期将 boolean 类型的数据转换为 int,使用1来表示 true,0表示false。JVM 支持 boolean 数组，但是是通过读写 byte 数组来实现的。
## 包装类型  
基本数据类型都有与其相对应的包装类型。  
包装类型中含有静态内部类(IntegerCache)作为缓存区。  
缓存区会缓存一定范围内的包装类对象(Integer[-128~127])，调用valueOf()方法时，如果值在范围内则 返回缓存区内的对象，如果超出范围则构造新的包装类。 

    public static void main(String[] args) {
    	Integer x = new Integer(1);
    	Integer y = new Integer(1);
    	System.out.println(x == y);
    	//false new Integer会重新创建一个对象
    	Integer z = Integer.valueOf(1);
    	Integer j = Integer.valueOf(1);
    	System.out.println(z == x);
    	//false z是IntegerCache中的new 出来的Integer
    	System.out.println(z == j);
    	//true j也是IntegerCache中的new 出来的Integer
    }
# 2. String #
##
String的不可变有两个方面  
一是String类是final的表示String类不可被继承  
二是String类中含有final 类型的属性。  

    public final class String
	    implements java.io.Serializable, Comparable<String>, CharSequence {
	    /** The value is used for character storage. */
	    private final char value[];
    }

## 不可变的好处 ##
1. 因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。
2. String的不可变保证了在多线程环境下的安全性。任何线程都无法改变String的值所有是线程安全的。
3. 为了实现String Pool
## String，StringBuilder，StringBuffer ##
String,StringBuffer是线程安全的。StringBuffer通过synchronized关键字实现线程安全。
## String Pool ##
String Pool保存着所有编译期确定的字面串变量，除此之外还可以调用str.intern()来将字符串保存到String Pool中。  
当调用str.intern()时，如果String Pool中存在和String相同的值则返回该字符串的引用，如果不存在则将当前字符串加入到String Pool中。

    String s1 = new String("aaa");	//String Pool中没有"aaa"时会创建2个对象，一个在
    String s2 = new String("aaa");	//堆，另一个在String Pool，之后返回堆中对象的引用
    System.out.println(s1 == s2);   // false
    String s3 = s1.intern();		
    String s4 = s1.intern();
    System.out.println(s3 == s4);   // true
	String s5 = "aa";				//会直接将字符串放到String Pool
Java7之前String Pool存放在永久代中。Java7开始将String Pool移动到了堆中。这是为了防止在大量使用字符串的情况下导致永久代出现OutOfMemoryError。
# 3.类加载过程  #
##
类加载的过程遵循：static代码块=static属性>非static代码块=非static属性>构造方法 的顺序。
当调用类的静态属性时不会导致非静态属性的加载。
并且加载外部类时不会导致内部类的加载，只有显式调用内部类时内部类才开始加载。  

    public class ClassLoadingSeq {
    	public static long outterStatic = System.currentTimeMillis();
    	public long outter = System.currentTimeMillis();
    	
    	static {
    		try {
    			Thread.sleep(1);
    		} catch (InterruptedException e) {
    			e.printStackTrace();
    		}
    		System.out.println("static block:"+System.currentTimeMillis());
    	}
    	
    	{
    		try {
    			Thread.sleep(1);
    		} catch (InterruptedException e) {
    			e.printStackTrace();
    		}
    		System.out.println("block:"+System.currentTimeMillis());
    	}
    
    	public ClassLoadingSeq() {
    		try {
    			Thread.sleep(1);
    		} catch (InterruptedException e) {
    			e.printStackTrace();
    		}
    		System.out.println("constructor method :"+System.currentTimeMillis());
    	}
    	
    	public static void main(String[] args) {
    		ClassLoadingSeq outter = new ClassLoadingSeq();
    		System.out.println("outter:"+outter.outter);
    		System.out.println("outterStatic:"+ClassLoadingSeq.outterStatic);
    		//static block:1565151660016
    		//block:1565151660018
    		//constructor method :1565151660020
    		//outter:1565151660016
    		//outterStatic:1565151660013
    		//注释掉前两句后执行结果如下
    		//static block:1565151752345
    		//outterStatic:1565151752343
    	}
    }
# 4.继承 #
##
## 抽象类和接口 ##
- 概念  
抽象类和抽象方法都被 abstract 修饰，如果一个类含有抽象方法那它一定是一个抽象类。  
抽象类和类之间的主要区别在于抽象类不能被实例化，只能通过子类继承才能完成实例化。
接口是一种特殊的抽象类。在Java 8 之前，它可以看成是一个完全抽象的方法，也就是说它不能实现任何类。  
在Java 8 之后，接口也允许实现默认的方法，这是因为不支持默认方法的接口的维护成本太高了。在Java 8 之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类。  
默认方法的使用和抽象类中的非抽象方法一样，可以被实现了接口的子类所调用。 
接口中的所有成员都是 public 的，所有字段都是 static final 的。  

	    public interface Interface {
	    	default void fun() {
	    		System.out.println("fun");
	    	}
	    }
	    public class Children implements Interface {
	    	public static void main(String[] args) {
	    		Children children = new Children();
	    		children.fun();
	    	}
	    }

- 比较  
抽象类和类之间是一种 IS-A 关系，它要求子类和抽象类之间满足里式替换原则。而接口和类之间是一种 LIKE-A 关系，只需要子类和接口有公共之处即可。  
一个类只能继承一个抽象类，但是可以实现多个接口。  
接口中的成员都是 public 的，抽象类的成员可以有多种访问权限。  
接口中的字段都是 static final ，抽象类和类一样。
## 重写与重载
重写指子类实现了一个与父类在方法声明上完全相同的一个方法。  
为了满足里氏替换原则，子类的方法返回值要小于父类，抛出的异常要小于父类，访问修饰限定符要大于父类。  
重载指：同一个类中实现了多个同名的方法。这些方法的参数类型、个数、顺序不同。重载和返回值的类型无关。
## 5.Object通用方法 ##
### 概览 ###
    public native int hashCode()
    
    public boolean equals(Object obj)
    
    protected native Object clone() throws CloneNotSupportedException
    
    public String toString()
    
    public final native Class<?> getClass()
    
    protected void finalize() throws Throwable {}
    
    public final native void notify()
    
    public final native void notifyAll()
    
    public final native void wait(long timeout) throws InterruptedException
    
    public final void wait(long timeout, int nanos) throws InterruptedException
    
    public final void wait() throws InterruptedException
### 重写equals时要注意哪些事情  ###
- 自反性	`x.equals(x);//true`
- 对称性	`x.equals(y)==y.equals(x);`
- 传递性	`if(x.equals(y)&&y.equals(z)) x.equals(z);//true`
- 一致性:多次调用 equals 返回的结果不变。
- 与null值比较 `x.equals(null);\\false`
- equals()比较相等的两个数它的 hashCode() 一定相等。而 hashCode() 相等的数不equals()比较不一定相等。Java中用到hash相关的集合时，我们可能希望将两个equals比较相等的两个对象当成一样的，只在hash中存储一个对象。而如果没有重写 hashCode() 就会在hash中存2个对象。
### 怎么重写 hashCode() ###
    @Override
	public int hashCode() {
		int result = 0;/设置一个随机的result
		result = result*31 + a;
		return result;
	}
## 关键字 ##
## 反射 ##
## 异常 ##
## 泛型 ##
## 注解 ##
>>>>>>> aaafd876e4311dcaeaa8f44a713fcce85383e9ae
## 特性 ##