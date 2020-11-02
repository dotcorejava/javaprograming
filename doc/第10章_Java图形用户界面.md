# 第10章 Java图形用户界面

## AWT 和 Swing
Java为图形用户界面(Grahi User Interface,GUI)提供的对象都保存在java Awt和javx.Swing两个包中,包内有相关的组件。GUI能够用图形的方式显示计算机操作界面,这样更加方便、直观。

### AWT
AWT是抽象窗口工具包(Abstract Window Toolkit)的英文缩写。抽象窗口工具包为开发者提供了建立图形用户界面的工具集。其主要功能如下:
1. 用户界面组件。
2. 事件处理模型。
3. 图形和图像工具。
4. 布局管理器。
5. 数据传送类。

### Swing
Swing的组件几乎都是轻量级的,与AWT组件不同的是,这些组件没有本地对等组件是由纯Java实现的,因此它们不依赖于操作系统。与AWT的重量级组件相比,Swing组件被称作轻量级组件。重量级组件是在本地的不透明窗体中绘制,而轻量级组件是在重量级组件的窗口中绘制。由于抛弃了基于本地对等组件的同位体体系结构,因此Swing不但在不同的平台上表现一致,而且还提供了本地组件不支持的特性。

>AWT是Swing的基础,AWT最初的设计也只是定位于小应用程序的简单用户界面。

## 窗体
```
import javax.swing.JFrame;

public class HelloWorld {
        public static void main(String[] args) {
                JFrame f = new JFrame("第一个窗体");
                f.setSize(400, 300);
                f.setLocation(100, 200);
                f.setVisible(true);
                f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        }
}
```

## 布局
- setBound 绝对定位
- FlowLayout 流式布局
- BorderLayout 边界布局
- GridLayout 网格布局
- CardLayout 卡片布局

## 组件
- JTextComponent 文本组件
    - JTextField 单行文本框
    - JPasswordField 密码文本框
    - JTextArea 多行文本框

## 事件处理
一个图形界面制作完成后,要想让每一个组件都发挥自己的作用,就必须对所有的组件进行事件处理,才能实现软件与用户的交互。常用的事件有窗体事件、动作事件、焦点事件、鼠标事件和键盘事件。在Swing编程中,依然使用最早的AWT事件处理方式。
```
import java.awt.BorderLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;

public class HelloWorld {
        public static void main(String[] args) {
                JFrame f = new JFrame("点击事件demo");
                JLabel l = new JLabel("欢迎登录");
                l.setHorizontalAlignment(JLabel.CENTER);
                JButton b = new JButton("登录");

                b.addActionListener(new ActionListener() {

                        @Override
                        public void actionPerformed(ActionEvent e) {
                                JButton button = (JButton) e.getSource();
                                if (e.getActionCommand().equals("登录")) {
                                        l.setText("已成功登录");
                                        button.setText("退出");
                                } else {
                                        l.setText("已安全退出");
                                        button.setText("登录");
                                }
                        }
                });

                f.add(l);
                f.add(b, BorderLayout.SOUTH);
                f.setBounds(100, 100, 230, 120);
                f.setVisible(true);
        }
}
```

```
import java.awt.BorderLayout;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JTextField;

public class HelloWorld {
        public static void main(String[] args) {
                JFrame f = new JFrame("点击事件demo");
                JLabel l = new JLabel("欢迎登录");
                l.setHorizontalAlignment(JLabel.CENTER);
                JButton b = new JButton("登录");

                b.addActionListener(e -> {
                        JButton button = (JButton) e.getSource();
                        if (e.getActionCommand().equals("登录")) {
                                l.setText("已成功登录");
                                button.setText("退出");
                        } else {
                                l.setText("已安全退出");
                                button.setText("登录");
                        }
                });
                JTextField t = new JTextField();
                t.setBounds(20, 30, 100, 30);
                t.addKeyListener(new KeyAdapter() {
                        @Override
                        public void keyTyped(KeyEvent e) {
                                if (e.getKeyChar() == 'q') {
                                        System.exit(1);
                                }
                        }

                });
                f.add(t);
                f.add(l);
                f.add(b, BorderLayout.SOUTH);
                f.setBounds(100, 100, 400, 300);
                f.setVisible(true);
        }
}
```

## 练习题
### 填空题
1. Java图形用户界面的基本组成部分是组件。组件是以__显示在屏幕上能进行__的对象。
2. 容器是一种特殊的__,其主要功能是容纳其他组件和容器。
3. 通过布局管理器可以使我们生成的图形用户界面具有良好的__。
4. Component类直接继承自__类,是一个抽象类。
5. Frame类用于建立标准的窗口,继承自__类。
6. AWT基于本地对等组件的__体系结构,而Swing是由纯Java实现的,其组件不依赖于__。
7. Swing的组件几乎都是__,而AWT的组件被称作__。
8. Swing不但在__上表现一致,而且还提供了__不支持的特性。
9. Swing采用MVC(Model-View-Controller)设计模式,即__。
10. Java AWT事件处理模型将__和__完全分开。
11. Java的事件处理采用__模式,即事件源可以把在其自身所有可能发生的事件分别授权给__。
12. 事件处理者一直监听__上发生的事件,一旦发现是其__就马上进行处理。这也是事件处理者被称作监听器的原因。
13. 除了实现事件监听器接口外,还可以通过__构造监听器。Java语言为一些监听器接口提供了__。
14. AWT事件可以分为两大类,即__和__。

### 选择题
1. 下面没有被Component类实现的接口是__。  
A. ImageObserver  
B. MenuContainer  
C. Serializable  
D. Clone
2. Frame类默认的布局管理器是__。  
A. BorderLayout  
B. FlowLayout  
C. CardLayout  
D. GridLayout
3. Frame类直接继承自下面哪个类__。  
A. Container  
B. Window  
C. Component  
D. Object
4. 下列哪个布局管理器会把加入的组件像卡片一样重叠放置,使用者第一次只能看到最上面的卡片__。  
A. BorderLayout  
B. FlowLayout  
C. CardLayout  
D. GridLayout
5. GridBagLayout布局管理器不限定加入组件的大小都相同,通过下面哪个类设置每个组件的大小__。  
A. GridBagConstraints  
B. GridLayout  
C. Frame  
D. Window
6. 下面Swing类中,没有继承JComponent类的是__。  
A. JFrame  
B. JComboBox  
C. JTable  
D. JTree
7. 下面Swing类中,没有实现Accessible接口的是__。  
A. JTree  
B. JTabbedPane  
C. JOptionPane  
D. JComponent
8. 在下列Swing组件中,哪个组件可以用于构建对话框__。  
A. JInternalFrame  
B. JOptionPane  
C. JFrame  
D. JTabbedPane
9. 在下列Swing组件中,哪个组件可以添加标签页__。  
A. JInternalFrame  
B. JOptionPane  
C. JFrame  
D. JTabbedPane
10. 在下列Swing组件中,哪个组件可以用来分隔窗体__。  
A. JComponent  
B. JSplitPane  
C. JFrame  
D. JTabbedPane
11. 与AWT有关的所有事件类都继承自AWTEvent类。下列哪个类不是继承自AWTEvent类__。  
A. EventObject  
B. KeyEvent  
C. ComponentEvent  
D. MouseEvent
12. 下列事件中不属于低级事件的是__。  
A. ContainerEvent  
B. ActionEvent  
C. MouseEvent  
D. FocusEvent
13. 下列事件中不属于高级事件的是__。  
A. AdjustmentEvent  
B. ItemEvent  
C. ComponentEvent  
D. TextEvent
14. 窗口被关闭触发的事件被封装在下列哪个类中__。  
A. WindowEvent  
B. AdjustmentEvent  
C. ItemEvent  
D. TextEvent
15. 滑动滚动条触发的事件被封装在下列哪个类中__。  
A. WindowEvent  
B. AdjustmentEvent  
C. ItemEvent  
D. TextEvent