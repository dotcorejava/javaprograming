# 第3章 Java面向对象编程

## 基本特性
- 封装性
- 继承性
- 多态性

## 类
>Java面向对象的核心组成是“类(class)”。

### 类修饰词
- public：被public修饰的类对所有类都是可见的,并且必须定义在以该类名为名字的Java文件中。
- abstract：被abstract修饰的类是抽象类。因为抽象类至少有一个成员方法需要在其子类中给出完整定义,所以抽象类不能被实例化。
- final：被final修饰的类不能被继承,或者说不能作为超类也不可能有子类,这样的类编译器会对其进行优化。
- 默认:如果没有指定类修饰词,就表示使用默认类修饰词。在这种情况下,其他类可以继承此类,同时在同一个包下的类可以访问引用此类。

### 成员修饰词 
- private:被private修饰的成员变量只对成员变量所在类可见。
- protected:被protected修饰的成员变量对成员变量所在类、该类同一个包下的类和该类的子类可见。
- public:被public修饰的成员变量对所有类可见。
- 默认:如果没有指定访问控制修饰词,就表示使用默认修饰词。在这种情况下,成员变量对成员变量所在类和该类同一个包下的类可见。
- static:被static修饰的成员变量仅属于类的变量,而不属于任何一个具体的对象。静态成员变量的值是保存在类的内存区域的公共存储单元,而不是保存在某一个对象的内存区间。任何一个类的对象访问它时,取到的都是相同的数据;任何一个类的对象修改它时,也都是对同一个内存单元进行操作。
- final:被final修饰的成员变量在程序的整个执行过程中都是不变的,可以用它来定义符号常量。
- **transient:被transient修饰的成员变量是暂时性变量。Java虚拟机在存储对象时不存储暂时性变量。在默认情况下,类中所有变量都是对象永久状态的一部分,当对象被存档时,这些变量同时被保存。**
- **volatile:被volatile修饰的成员变量不会被编译器优化,这样可以减少编译的时间。这个修饰词并不常用。**
- abstract:被abstract修饰的成员方法称作抽象方法。抽象方法是一种仅有方法头,没有方法体和操作实现的一种方法。
- native:被native修饰的成员方法称作本地方法。本地方法的方法体可以用像C语言这样的高级语言编写。
- synchronized:被synchronized修饰的成员方法用于多线程之间的同步。这个修饰词在后面章节中会有更详细的说明。
```
public class User {
    private String name;
    private int age;

    public User() {
    }

    public User(String name) {
        this.name = name;
    }

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public static String getClassName() {
        return "User";
    }

    /**
     * InnerUser
     */
    public class InnerUser {
        public String getSuperName() {
            return getName();
        }

    }
}
```

## 继承
1. 子类继承父类所有的属性和方法,同时也可以在父类继承上增加新的属性和方法。
2. 子类不继承父类的构造器。
3. 子类可以继承父类中所有的可被子类访问的成员变量和方法
>子类方法不能覆盖父类中声明为final或static的方法。  
>子类方法不能覆盖父类中声明为final或static的方法。  
子类方法必须覆盖父类中声明为abstract的方法(接口或抽象类)。
```
public class User {
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

class AdminUser extends User {
    private int role;

    public int getRole() {
        return role;
    }

    public void setRole(int role) {
        this.role = role;
    }

    public AdminUser(String name, int age, int role) {
        super(name, age);
        this.role = role;
    }

    public AdminUser getAdminUser() {
        return new AdminUser(this.getName(), this.getAge(), this.getRole());
    }
}
```

### final 关键字
- final 类不能被继承
- final 方法不能被子类覆盖
- final 变量声明时赋值，不能被修改

### 抽象类
被abstract修饰词修饰的类称为抽象类。抽象类包含抽象方法的类。  

抽象方法:只声明未实现的方法,抽象类必须被继承。如果子类不是抽象类,就必须覆写抽象类中的全部抽象方法。

>抽象类是不完整的类,不能通过构造方法被实例化。但这不代表抽象类不需要构造方法,其构造方法可以通过前面介绍的super关键字在子类中调用。
```
abstract class User {
    private String name;

    protected User(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    abstract int getNum();
}

class AdminUser extends User {
    private int age;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public AdminUser(String name, int age) {
        super(name);
        this.age = age;
    }

    @Override
    int getNum() {
        return 10;
    }
}
```

### 接口
接口是另一种引用类型。接口的修饰词只有public和默认两个,含义与类修饰符相同。接口名的命名规则也与类名相同。因为接口中变量的修饰词只能是public、final、static,所以不用显式地使用修饰词。又因为接口中变量的修饰词是public、final、static,所以在接口中声明的都是常量。接口中的方法都没有方法体,除了定义的常量以外也没有变量。接口中方法的修饰词只能是public,默认也是public。  
另外,接口也有继承机制并支持多继承,可以使用extends关键字继承其他接口。

```
public interface User {
    public static final String NAME = "User";

    int getNum();
}

class AdminUser implements User {

    @Override
    public int getNum() {
        return 10;
    }

}
```

### 匿名内部类
匿名内部类就是没有名字的内部类,经常被应用于Swing程序设计中的事件监听处理。
```
public class ClassDemo {
    public static void main(String[] args) {
        new ButtonAction(){
            public void click(){
                System.out.println(" 这是匿名类,但谁也无法使用它! ");
            }
        }
    }
}
```

## 多态
- 方法的重载和覆盖
- 对象的动态性

>Object是Java中唯一一个没有父类的类,是Java最顶层的父类。
Object()、equals(Object)、toString()、hashCode()

### 包装类
|基本数据类型|包装类|
|---|---|
|byte|Byte|
|short|Short|
|int|Integer|
|long|Long|
|float|Float|
|double|Double|
|char|Character|
|boolean|Boolean|

- 装箱拆箱
Integer n = 12; // 装箱
int m = n; // 拆箱

>重写 equals() 和 toString() 方法覆盖父类方法。
  
## 练习
1. 编写一个程序,得出三个不同盒子的体积。将每个盒子的高度、宽度和长度参数的值传递给构造方法,计算并显示体积。  

2. 编写一个程序,显示水果的定购详情。定义一个带有参数的构造方法,这些参数用于存放产品名、数量和价格。此程序接受并输出构造方法的参数,即三种不同的水果。  

3. 编写一个程序,用于创建一个名为Employee的父类和两个名为Manager和Director的子类。Employee类包含三个属性和一个方法,属性为name、basic和address,方法为show(),用于显示这些属性的值。Manager类有一个称为department的附加属性,Director类有一个称为transportAllowance的附加属性,创建Manager和Director类的对象并显示其详细信息。  

4. 编写一个程序,用于重写父类Addition中名为add()的抽象方法。add()方法在NumberAddition类中将两个数字相加,在TextConcatenation类中连接两个Strings(字符串)。声明属性并在父类Addition的构造方法中初始化属性。