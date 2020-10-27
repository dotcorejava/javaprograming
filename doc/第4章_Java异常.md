# 第4章 Java异常

## Throwable 类
- 子类 Error  
Error类特指应用程序在运行期间发生的严重错误,如虚拟机内存用尽、堆栈溢出、动态链接失败等,对于这类错误导致的应用程序中断,程序无法预防和恢复。
- 子类 Exception  
Exception类是指一些可以被捕获且可能恢复的异常情况,如数组下标越界、数字被零除产生异常、输入/输出异常等。  
Exception类又分为运行时异常和非运行时异常。RuntimeException及其子类都属于运行时异常,如数字被零除产生异常、数组下标越界异常等。除了运行时异常以外的Exception子类都是非运行时
异常。

### 异常捕获 try-catch-catch-finally
```
public class HelloWorld {
        public static void main(String args[]) {
                try {
                        A a = new A();
                        a.sayHi("java.io.PrintStream");
                } catch (Exception e) {
                        System.out.println(e);
                } finally {
                        System.out.println("done");
                }
        }
}

class A {
        public void sayHi(String className) throws ClassNotFoundException {
                Class.forName(className);
        }
}
```

### 异常抛出 throws-throw
```
public class HelloWorld {
        public static void main(String args[]) throws Exception {
                try {
                        A a = new A();
                        a.sayHi("");
                } catch (Exception e) {
                        System.out.println(e);
                        throw e;
                } finally {
                        System.out.println("done");
                }
        }
}

class A {
        public void sayHi(String className) throws ClassNotFoundException {
                Class.forName(className);
        }
}
```

```
public class HelloWorld {
        public static void main(String args[]) {
                try {
                        throwCE();
                } catch (CE ce) {
                        System.out.println(ce.getMessage());
                        ce.printStackTrace();
                } finally {
                        System.out.println("done");
                }
        }

        public static void throwCE() throws CE {
                throw new CE();
        }
}

class CE extends Throwable {

        private static final long serialVersionUID = 1L;

        @Override
        public void printStackTrace() {
                System.out.println("CE Stack Trace");
        }

        @Override
        public String getMessage() {
                return "CE Message";
        }
}
```

## 练习
### 填空题
1. Java 中，异常分为__和__两类。
2. 异常是在运行时程序代码序列中产生的一种__、__。
3. 在 Java 中，每个异常都是一个__，它是__或其子类的实例。
4. Throwable 类有两个重要的子类：__和 __。
5. Exception 又分为__异常和__异常。

### 选择题
1. 下列哪种方法不是 Throwable 类的构造方法__。  
A. Throwable()  
B. Throwable(String message)  
C. Throwable(Throwable cause, String message)  
D. Throwable(Throwable cause)  

2. 下列异常类中不是继承自RuntimeException类的是__。  
A. ArithmticException  
B. NullPointerException  
C. IndexOutOfBoundException  
D. FileNotFoundException  

3. 下列不是用来捕获处理异常的关键字是__。  
A. throws  
B. try  
C. catch  
D. finally  

4. 下列关键字哪个是用来在方法名后声明抛出异常的__。  
A. throw  
B. throws  
C. enum  
D. final  

5. 在抛出异常时, 会自动调用这个异常的是哪个方法__。
A. clone()  
B. getMessage()  
C. fillInStackTrace()  
D. toString()

### 问答题
1. 简述在没有异常机制的程序语言中如何进行错误识别和处理。
2. 简述 java 异常处理机制（两种方式）。

### 编程练习
编写一个自定义异常类，要求继承 Execption 类，在 main 方法中抛出这个异常并进行捕获处理。