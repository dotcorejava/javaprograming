# 第6章 Java集合框架

## Collection 接口
Collection接口是整个Java集合框架的基石,定义了集合框架中一些基本的方法。在某种意义上可以把Collection看成是动态的数组,一个对象的容器。通常把放入Collection中的对象称作元素。

### Collection 子接口 List
List接口继承Collection接口并在其基础上进行扩充,有两个非常重要的实现类:ArrayList和LinkedList。

- List 接口实现类 ArrayList  
ArrayList类是List接口的重要实现类,就像一个动态分配的数组,每次将新添加的对象放在最后。

- List 接口实现类 LinkedList  
LinkedList类是List接口的另一个重要的实现类。因为LinkedList类的底层实现是链表,所以便于将新加入的对象插入指定位置。

### Collection 子接口 Set
Set接口继承Collection接口,自身并没有引入新的方法,只是不允许集合中存在相同的元素。每个具体的Set实现类依赖添加对象的equals()方法检查唯一性。

- Set 接口实现类 HashSet  
HashSet类是Set接口实现类中最常用的一个。因为HashSet类通过Hash算法进行存储,所以具有快速定位元素的特点。

## Map 接口
Map看作是用于存储关键字/值对映射的容器,与Set接口相似,一个Map容器的关键字对象不允许重复。只有保证关键字对象的唯一性,才能完成根据关键字
对象获取值对象的功能。

- Map 接口实现类 HashMap  
HashMap是Map接口的重要实现类,在Java程序设计中经常用到。因为HashMap也采用Hash算法,所以可以快速定位关键字对象。

### Map.Entry 接口
如果说Map接口是一个用于存储关键字/值对映射的容器,那么Map.Entry接口就是用于描述存储在Map容器中的一个关键字/值对映射。
```
import java.util.HashMap;
import java.util.Map;
import java.util.Map.Entry;

public class HelloWorld {
        public static void main(String[] args) {
                Map<String, String> m = new HashMap<>();
                m.put("a", "b");
                m.put("c", "d");
                for (Entry<String, String> entry : m.entrySet()) {
                        System.out.printf("%s: %s\n", entry.getKey(), entry.getValue());
                }

                m.forEach((k, v) -> {
                        System.out.printf("%s: %s\n", k, v);
                });
        }
}
```

## Iterator 接口
Iterator接口用于对集合容器进行向前的单方向遍历,通常称作迭代器。

### Iterator 子接口 ListIterator
ListIterator接口与Iterator接口类似,不同之处在于ListIterator用于对List容器进行双向遍历。

>在进行程序设计时,建议使用接口作为方法的返回值。


## 练习
### 填空题
1. ArrayList类通过__方法获得迭代器,从而对所有元素进行遍历。
2. Map接口通过__方法返回Map.Entry对象的视图集,即映像中的关键字/值对。
3. ArrayList类通过__方法增加容量。
4. ArrayList类通过__方法设置其容量为列表当前大小。
5. HashMap类通过__方法返回映像中所有值的视图集。

### 选择题
1. 下列集合类中,哪个可以使用ListIterator进行遍历。__  
A. ArrayList  
B. List  
C. HashMap  
D. LinkedList
2. 下列哪个集合类使用链表作为底层实现的方式。__  
A. ArrayList  
B. LinkedList  
C. HashSet  
D. HashMap
3. 下列哪个集合类可以用于存储关键字/值对映像。__  
A. Map  
B. Map.Entry  
C. HashMap  
D. HashSet
4. 下列哪个集合类不允许存在相同的元素。__  
A. HashSet  
B. Set  
C. ArrayList  
D. LinkedList
5. 下列哪个接口没有继承Collection接口。__  
A. Map  
B. HashMap  
C. Set  
D. List

### 问答题
1. 简述Java集合的作用。
2. 简述Collection、Map、List、HashMap、Set、ArrayList的关系。

### 编程练习
编写一个方法,实现将存储在HashMap中的数据转存到Collection类型的集合中。