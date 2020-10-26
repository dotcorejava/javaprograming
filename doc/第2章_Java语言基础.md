# 第2章 Java语言基础

## 注释
- 单行注释 `//`
- 多行注释 `/* */`
- 文档注释 `/** */`

 ## 标识符命名规则
 |元素|规范|示例|
 |---|---|---|
 |类名、接口名|首字母大写|Person、SystemManager|
 |变量名、数组名|Camel规则，小写开头|age、userInfo|
 |函数名（方法名）|Camel规则，小写开头|print、getName|
 |包名|全部小写|com.mdjdot.zz|
 |常量名|全部大写|MAX_VALUE|

 ## 变量和常量声明
```
int m;
m = 10;

int n = 20;

final PI = 3.14f;

int[] array = new int[3];
array = new int[] { 1, 2, 3 };
int nums[] = new int[] { 1, 2, 3, 4 };
int[] nums = { 1, 2, 3 };
int[][] mn = new int[][]{new int[3],new int[4]};
```

## Java 数据类型
- 原始类型
    1. 整数类型：byte、short、int、long
    2. 浮点类型：float、double
    3. 字符类型：char  
    4. 布尔类型：boolean
- 引用类型
    1. 数组
    2. 类
    3. 接口
>char即字符型,Java使用Unicode码代表字符,这一点决定了Java中char所占位数不同于C/C++的8位而是16位。因为char是无符号的,所以取值范围为0~65535。

>数值类型支持自动转换和强制转换。

## 算术运算符
- +、+=、i++、++i
- -、-=、i--、--i
- *、*=
- /、/=
- %、%=

>对于byte类型、short类型和char类型的变量,上面的等价关系并不成立。对于这三种类型的变量,执行var1=var+1;语句会产生错误,因为常数1默认为int类型,根据前面提到的表达式中类型提升原则,这样的语句相当于将int类型隐式地转换为范围低于它的类型。而这样做是不可能通过编译的,必须进行类型的强制转换才行。

## 位运算符
- ～
- &、&=
- |、|=
- ^、|=
- `>>`、>>= （符号位不变，空位补0）
- <<、<<= （符号位不变，空位补0）
- `>>>`、>>>= (右移，符号位补0，空位补1)

## 关系运算符
- ==
- !=
- >
- <
- `>=`
- <=

## 三元运算符
boolean?var1:var2;

## 流程控制
### 选择 if-else if-else
```
int n = 10;
if (n > 90) {
        System.out.println("A");
} else if (n > 80) {
        System.out.println("B");
} else if (n > 80) {
        System.out.println("B");
} else {
        System.out.println("C");
}
```

### 分支 switch-case-default
```
int m = 5;
switch (m) {
        case 1:
                System.out.println("A");
                break;
        case 3:
                System.out.println("B");
                break;
        case 5:
                System.out.println("C");
                break;
        default:
                System.out.println("E");
                break;
}
```

### 循环 for、while、do-while
```
for (int i = 0; i < 10; i++) {
        System.out.println(i);
}

int j = 1;
for (; j < 10; j++) {
        System.out.println(j);
}

int k = 1;
for (; k < 10;) {
        System.out.println(k);
        k++;
}

int l = 1;
for (;;) {
        if (l < 10) {
                if (l % 2 == 0) {
                        l++;
                        continue;
                }
                System.out.println(l);
                l++;
        } else {
                break;
        }
}

int[] nums = new int[] { 1, 2, 3, 4, 5 };
for (int i : nums) {
        System.out.println(i); 
}

int m = 1;
while (m < 10) {
        System.out.println(m);
        m++;
}

int n = 1;
do {
        System.out.println(n);
        n++;
} while (n < 10);
```

## 方法
```
public static int numPlus(int i) {
        return ++i;
}

public static int numPlus(int i, int j) {
        return i + j;
}

public static void main(String args[]) {
    int[] nums = new int[3];
    for (int i : nums) {
            System.out.println(i);
    }
    updateArray(nums);
    for (int i : nums) {
            System.out.println(i);
    }
}

private static void updateArray(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
            nums[i]++;
    }
}
```
>返回值类型不是区分方法的条件

>递归调用是一种特殊的调用形式,属于方法的自身调用,在开发中应尽量避免使用。因为递归调用在操作时如果处理不好,就可能会出现内存的溢出,所以要谨慎使用

## 练习
1. 绘制柱状图:读入5个数,每个数介于1~15,每个*代表一个数字,显示出柱状图。例如:
    ```
    5,12,7,10,8
    *****
    ************
    *******
    **********
    ********
    ```

2. 读入一个月份,假设该月份1日是周3,请显示该月星期历,如果是2月份,就则认为是28天。

    |日|一|二|三|四|五|六|
    |---|---|---|---|---|---|---|
    ||||1|2|3|4|
    |5|6|7|8|9|10|11|
    |12|13|14|15|16|17|18|
    |19|20|21|22|23|24|25|
    |26|27|28|29|30|31||

3. 在排序好的数组中添加一个数字,并将添加后的数字插入到数组合适的位置。