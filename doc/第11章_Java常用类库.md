# 第11章 Java常用类库

## StringBuffer 类
StringBuffer类支持的方法大部分与String类似,因为StringBuffer类在开发中可以提升代码的性能,所以使用较多。Java为了保证用户操作的适应性,在StringBuffer类中定义的方法名称大部分都与String一样。

## Runtime 类
在Java中,Runtime类表示运行时的操作类,是一个封装了JVM进程的类,每一个JVM都对应着一个Runtime类的实例,此实例由JVM运行时为其实例化。
```
import java.io.IOException;

public class HelloWorld {
        public static void main(String[] args) {
                Runtime run = Runtime.getRuntime();
                System.out.println(run.availableProcessors());
                System.out.println(run.freeMemory());
                System.out.println(run.maxMemory());
                Process p = null;
                try {
                        p = run.exec("ls -d");
                } catch (IOException e) {
                        e.printStackTrace();
                }
                p.destroy();
                System.out.println(p.exitValue());
        }
}
```

## System 类
System类是一些与系统相关属性和方法的集合。在System类中所有属性都是静态的,要想引用这些属性和方法,直接使用System类调用即可。

## Math 类
Math类是数学操作类,其提供了一系列的数学操作方法,包括求绝对值、三角函数等。因为在Math类中提供的方法都是静态的,所以直接由类名称调用即可。

## 练习题
### 填空题
1. 每个Java基本类型在java.lang包中都有一个相应的包装类,把基本类型数据转换为对象。其中,包装类Integer是__的直接子类。
2. 包装类Integer的静态方法可以将字符串类型的数字“123”转换为基本整型变量n,其实现语句是:___Integer.parseInt(“123”) __。
3. 使用java.lang包中的___StringBuffer/StringBuilder类创建一个字符串对象,它代表一个字符序列可变的字符串,可以通过相应的方法改变这个字符串对象的字符序列。
4. StringBuilder类是StringBuffer类的替代类,两者的共同点是都是,可变长度字符串,其中线程安全的类是____StringBuffer____。
5. 使用Math.random()返回带正号的double值,该值大于等于0.0且小于1.0。使用该函数生成[30,60]之间的随机整数的语句是___(int)(Math.random()*31)+30。

### 选择题
1. 以下选项中,关于int和Integer的说法错误的是()。  
A. int是基本数据类型,Integer是int的包装类,是引用数据类型  
B. int的默认值是0,Integer的默认值也是0  
C. Integer可以封装了属性和方法提供更多的功能  
D. Integer i=5;该语句在JDK1.5之后可以正确执行,使用了自动拆箱功能
2. 分析如下Java代码,该程序编译后的运行结果是()。
    ```
    public static void main(String[ ] args) {
        String str=null;
        str.concat("abc");
        str.concat("def");
        System.out.println(str);
    }
    ```
    A. Null  
    B. Abcdef  
    C. 编译错误  
    D. 运行时出现NullPointerException异常
3. 以下关于String类的代码执行结果是()。
    ```
    public class Test2 {
    public static void main(String args[]) {
        String s1 = new String("bjsxt");
        String s2 = new String("bjsxt");
        if (s1 == s2)
        System.out.println("s1 == s2");
        if (s1.equals(s2))
        System.out.println("s1.equals(s2)");
        }
    }
    ```
    A. s1==s2  
    B. s1.equals(s2)  
    C. s1==s2 s1.equals(s2)  
    D. 以上都不对
4. 以下关于StringBuffer类的代码执行结果是()。
    ```
    public class TestStringBuffer {
        public static void main(String args[]) {
            StringBuffer a = new StringBuffer("A");
            StringBuffer b = new StringBuffer("B");
            mb_operate(a, b);
            System.out.println(a + "." + b);
        }
        static void mb_operate(StringBuffer x, StringBuffer y) {
            x.append(y);
            y = x;
        }
    }
    ```
    A. A.B  
    B. A.A  
    C. AB.AB  
    D. AB.B