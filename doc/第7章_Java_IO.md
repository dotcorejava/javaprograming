# 第7章 Java IO

>Java的核心库java.io提供了全面的IO接口,包括文件读写、标准设备输出等。Java中IO是以流为基础进行输入/输出的,所有数据被串行化写入输出流,或者从输入流读入。

## File 类
```
import java.io.File;
import java.io.IOException;

public class HelloWorld {
        public static void main(String[] args) {
                File d = new File("./fdata")
                
                File f = new File("./dat");
                System.out.println(File.pathSeparator);
                System.out.println(File.separator);
                if (!f.exists()) {
                        try {
                                f.createNewFile();
                        } catch (IOException e) {                                
                                e.printStackTrace();
                        }
            }
        }
}
```

## RandomFileAccess 类
RandomFileAccess 类可以读写文件。
```
import java.io.File;
import java.io.RandomAccessFile;

public class HelloWorld {
        public static void main(String[] args) {
                File f = new File("./fdata/dat");

                try {
                        RandomAccessFile raf = new RandomAccessFile(f, "rw");
                        long fileLength = raf.length();
                        long filePoint = 0l;
                        while (filePoint < fileLength) {
                                System.out.println(raf.readLine());
                                filePoint = raf.getFilePointer();
                        }
                        raf.close();

                } catch (Exception e) {
                        e.printStackTrace();
                }
        }
}
```

## 字节流与字符流
流的操作主要有字节流和字符流两大类,两类都有输入和输出操作。在字节流中输入数据使用的是InputStream类,输出数据使用的是OutputStream类。在字符流中输入数据使用是Reader类,输出数据使用是Writer类。

### 字节流
字节流主要操作byte类型数据。以byte数组为准,主要操作类是InputStream和OutputStream。
```
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;

public class HelloWorld {
        public static void main(String[] args) {
                File f = new File("./dat");

                try {
                        FileOutputStream out = new FileOutputStream(f);
                        for (int i = 1; i < 11; i++) {
                                out.write(("第" + i + "行\n").getBytes());
                        }
                        out.close();

                        byte[] content = new byte[1024];
                        FileInputStream in = new FileInputStream(f);
                        int len = in.read(content);
                        System.out.println(new String(content, 0, len));
                        in.close();

                } catch (Exception e) {
                        e.printStackTrace();
                }
        }
}
```

### 字符流
Java中的一个字符占两个字节。Java提供了两个专门操作字符流的类:Reader和Writer。这两个类是字符流的抽象类,定义了字符流读取和写入的基本方法,各个子类会依其特点实现或覆盖这些方法。
```
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.Reader;
import java.io.Writer;

public class HelloWorld {
        public static void main(String[] args) throws IOException {
                File f = new File("./dat");
                try (Writer w = new FileWriter(f, true)) {
                        for (int i = 1; i < 11; i++) {
                                w.write("第" + i + "行\n");
                        }
                }

                try (Reader r = new FileReader(f)) {
                        char[] data = new char[1024];
                        int start = 0;

                        while (true) {
                                int n = r.read();
                                if (n == -1) {
                                        break;
                                }
                                data[start] = (char) n;
                                start++;
                        }
                        System.out.println(new String(data, 0, start));
                }

        }
}
```
>字节流与字符流在操作上非常类似,二者除了代码不同之外,最大的差别是操作时是否使用缓冲区(可理解为一段特殊的内存)。字节流在操作时不使用缓冲区,是文件本身直接操作的,而字符流在操作时使用缓冲区,即先通过缓冲区再操作文件。

### 字节流-字符流转换
- OutputStreamWriter类:将输出的字符流转换为字节流,即将一个字符流的输出对象变为字节流的输出对象。
- InputStreamReader类:将输入的字节流转换为字符流,即将一个字节流的输入对象变为字符流的输入对象。

### 打印流
打印流提供了非常方便的打印功能,可以打印任何数据类型,如小数、整数、字符串等。打印流是输出信息最方便的类,主要包含字节打印(PrintStream)和字符打印流(PrintWriter)。本节主要通过字节打印流(PrintStream)进行讲解。

### 管道流
管道流的主要作用是可以进行两个线程间的通信,分为管道输出流(PipeOutputStream)和管道输入流(PipedInputStream)。如果要进行管道输出,就必须把输出流连到输入流上。

### BufferedReader 类和 BufferedWriter 类
BufferedReader类用于从缓冲区中读取内容,所有的输入字节数据都将放在缓冲区;BufferedWriter类用于写入数据到缓冲区。  
BufferedReader类是Reader类的子类,使用该类可以以行为单位读取数据;BufferedWriter类是Writer类的子类,该类可以以行为单位写入数据。

### 数据操作流
Java io包中提供了两个与平台无关的数据操作流,分别为数据输出流(DataOutputStream)和数据输入流(DataInputStream)。通常数据输出流会按照一定的格式将数据输出,再通过数据输入流按照一定的格式将数据读入,这样方便对数据进行处理。

### 对象流
Java提供了ObjectInputStream与ObjectOutputStream类读取和保存对象,它们分别是对象输入流和对象输出流。

### Scanner 类
Scanner类是JDK1.5新增的类,是java.util包中的类。该类用于实现用户的输入,是一种只要有控制台就能实现输入操作的类。如果程序操作数据流只为读取文本数据,那么建议使用Scanner类实现具体操作。

## 练习
1. 编写一个程序,从名为handson.txt的文件中读取并显示用户名和密码。如果源文件不存在,就显示相应的错误信息。
2. 编写一个程序,接收从键盘输入的数据,并把从键盘输入的内容写到handsoninput.txt文件中,如果输入“quit”,程序就结束。
