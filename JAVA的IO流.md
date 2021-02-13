---
title: "JAVA的IO流"
date: 2020-07-14T09:59:01+08:00
draft: true
---

# IO流

## File

* File：它是文件和目录路径名的抽象表示
  * 文件和目录是可以通过File封装成对象的
  * 对于File而言，其封装的并不是一个真正存在的文件，仅仅只是一个路径而已，它可以是存在的，也可以是不存在的，将来要通过具体的操作把这个路径的内容转换为具体存在的



| 方法名                           | 说明                                                       |
| -------------------------------- | ---------------------------------------------------------- |
| File(String pathname)            | 通过将给定的路径名字符串转换为抽象路径名来创建新的File实例 |
| File(String parent,String child) | 从父路径名字符串和子路径名字符串创建新的File实例           |
| File(File parent,String child)   | 从父抽象路径名和子路径名字创建新的FIle实例                 |

### File类的创建功能

| 方法名                           | 说明                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| public boolean createnewFile（） | 当具有该名称的文件不存在时，创建一个由该抽象路径名命名的新空文件 |
| public boolean mkdir()           | 创建由此抽象路径名命名的目录                                 |
| public boolean mkdirs()          | 创建由此抽象路径名命名的目录，包括任何必需但不存在的父目录   |

### File类判断和获取功能

| 方法名                          | 说明                                                     |
| ------------------------------- | -------------------------------------------------------- |
| public boolean isDirectory()    | 测试此抽象路径名表示的File是否为目录                     |
| public boolean isFile()         | 测试此抽象路径名表示的File是否为文件                     |
| public boolean exists()         | 测试此抽象路径名表示的File是否存在                       |
| public String getAbsolutePath() | 返回此抽象路径名的绝对路径字符串                         |
| public String getPath()         | 将此抽象路径名转换为路径名字符串                         |
| public String getName()         | 返回由此抽象路径名表示的文件或目录的名称                 |
| public String[] list()          | 返回此抽象路径名表示的目录中的文件和目录的名称字符串数组 |
| public File[] listFiles()       | 返回此抽象路径名表示的目录中的文件和目录的File对象数组   |

### File类删除功能

| 方法名                  | 说明                               |
| ----------------------- | ---------------------------------- |
| public boolean delete() | 删除由此抽象路径名表示的文件或目录 |

* 绝对路径和相对路径的区别
  * 绝对路径：完整的路径名，不需要任何其他信息就可以定位它所表示的文件，例如：E:\\demo\\java.txt
  * 相对路径：必须使用取自其他路径名的信息进行解释。例如：myFile\\java.txt

* 删除目录时的注意事项：
  * 如果一个目录有内容（目录、文件），不能直接删除，应该先删除目录中的内容，最后才能删除目录。

***

## 字节流

* IO流概述
  * IO：输入/输出（Input/Output）
  * 流：是一种抽象概念，是对数据传输的总称，也就是说数据在设备间的传输称为流，流的本质是数据传输
  * IO流就是用来处理设备间数据传输问题的
    * 常见的应用：文件复制，文件上传，文件下载

* IO流分类

  * 按照数据的流向

    * 输入流：读数据

    * 输出流：写数据

  * 按照数据类型来分
    * 字节流
      * 字节输入流；字节输出流
    * 字符流
      * 字符输入流；字符输出流

  > 一般来说，我们说IO流的分类是按照***数据类型***来分的

* 那么什么时候该使用哪种流呢？
  
  * 如果数据通过Windows自带的记事本打开，我们可以看懂里面的内容，就用字符流，否则使用字节流，如果不知道使用哪种类型的流，就用字节流。

### 字节流写数据

* 字节流抽象基类
  * InputStream：这个抽象类是表示字节输入流的所有类的超类
  * OutputStream：这个抽象类是表示字节输出流的所有类的超类
  * 子类名特点：子类名称都是以其父类名作为子类名的后缀

* FileOutputStream：文件输出流用于将数据写入File
  * FIleOutputStream(String name)：创建文件输出流以指定的名称写入文件

* 使用字节输出流写数据的步骤：
  * 创建字节输出流对象（调用系统功能创建了文件，创建字节输出流对象，让字节输出流对象指向文件）
  * 调用字节输出流对象的写数据方法
  * 释放资源（关闭此文件输出流并释放与此相关联的任何系统资源）

```java
public class Demo{
    public static void main(String[] args) throws IOException{
        FileOutputStream fo = new FileOutputStream("D:\\Demo\\java.txt");//此处要处理异常
        /*
           以上实例化做了三件事情
             1.调用系统功能创建了文件
             2.创建了字节输出流对象
             3.让字节输出流对象指向创建好的文件
        */
        
        fo.write(102)//将指定的字节写入此文件输出流，同时此处也要处理异常
        //释放资源
        fo.close();
    }
}
```

* 字节流写数据的三种方式

| 方法名                               | 说明                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| void write(int b)                    | 将指定的字节写入此文件输出流，一次写一个字节数据             |
| void write(byte[] b)                 | 将b.length字节从指定的字节数组，写入此文件输出流，一次写一个字节数组数据 |
| void write(byte[] b,int off,int len) | 将len字节从指定的字节数组开始，从偏移量off开始写入此文件输出流，一次写一个字节数组的部分数据 |

```java
//关于字节流写数据的两个小问题
//问题1.如何换行
//问题2.如何增加新的内容而旧的内容不会被修改？
public class Demo{
    public static void main(String[] args) throws IOException{
        //创建字节输出流对象
        //FileOutputStream fo = new FileOutputStream("D:\\Demo\\java.txt");
        /*
          public FileOutputStream (String name,boolean append)
                 创建文件输出流以指定的名称写入文件
                 如果第二个参数为true，则字节将写入文件的末尾而不是开头
         */
        FileOutputStream fo = new FileOutputStream("D:\\Demo\\java.txt",true);
        
        /*
           换行：
           windows：\r\n
           linux: \n
           mac: \r
        */
        
        for(int i =0;i<10;i++){
              fo.write("hhhh".getBytes());
            fo.write("\r\n".getBytes());
        }
        
        fo.close();
    }
}
```

### 字节流写数据加异常处理

> finally : 在异常处理时提供finally块来执行所有清除操作，比如说IO流中的释放资源
>
> 特点：被finally控制的语句一定会执行，除非JVM退出

```java
try{
      //可能出现异常的代码
}catch(异常类名  变量名){
     //异常处理的代码
}finally{
      //一定会执行的操作（释放资源等操作）
}
```

```java
public class Demo{
    public static void main(String[] args) throws IOException{
        
        FileOutputStream fo = null;
        try{
            FileOutputStream fo = new FileOutputStream("D:\\Demo\\java.txt");
      
             fo.write(102);
        }catch(IOException e){
            e.printStackTrace();
        }finally{
            //释放资源
            if(fo != null){
                try{
                    fo.close();
                }catch(IOException e){
                    e.printStackTrace();
                }                
            }           
        }         
    }
}
```

***

### 字节流读数据

```java
public class Demo{
    public static void main(String[] args){
        //从文件系统中的文件获取输入字节
        FileInputStream fis = new FileInputStream("D:\\Demo\\java.txt");
        
        //获取字节
        int read;
        while((read = fis.read()) != -1){//read()方法，当读取到最后一个字节返回-1
            System.out.print((char)read);
        }
        
        //关闭资源
        fis.close();2
    }
}
```

```java
//一次读取多个字节数据
public class Demo{
    public static void main(String[] args){
        //从文件系统中的文件获取输入字节
        FileInputStream fis = new FileInputStream("D:\\Demo\\java.txt");
        
        //定义字节数组
        byte[] bys = new byte[1024];//通常定义为1024及其整数倍，用来存放读取的字节数据
        int read；
        while((read = fis.read(bys)) != -1){//read()方法，当读取到最后一个字节返回-1
            System.out.print(new String(bys,0,len));
        }
        
        //关闭资源
        fis.close();
    }
}
```

```java
//图片的复制
public class Demo{
    public static void main(String[] args){
        //从文件系统中的文件获取输入字节
        FileInputStream fis = new FileInputStream("D:\\Demo\\java.png");
        
        //输出对象
        FileOutputStream fos = new FileOutputStream("D:\\Demo\\java1.png");
        //定义字节数组
        byte[] bys = new byte[1024];//通常定义为1024及其整数倍，用来存放读取的字节数据
        int len；
        while((len = fis.read(bys)) != -1){//read()方法，当读取到最后一个字节返回-1
            fos.write(bys,0,len);
        }
        
        //关闭资源
        fis.close();
        fos.close();
    }
}
```

***

### 字节缓冲流

* 字节缓冲流
  * BufferOutputStream；该类实现缓冲输出流，通过设置这样的输出流，应用程序可以向底层输出流写入字节，而不必为写入的每个字节导致底层系统的调用
  * BufferedInputStream：创建BufferedInputStream将创建一个内部缓冲区数组，当从流中读取或跳过字节时，内部缓冲区将根据需要从所包含的输入流中重新填充，一次很多字节

* 构造方法
  * 字节缓冲输出流：BufferedOutputStream(OutputStream out)
  * 字节缓冲输入流：BufferedInputStream(InputStream in)

* 为什么构造方法需要的是字节流，而不是具体的文件或者路径呢？
  * 字节缓冲流仅仅提供缓冲区，而真正读写数据还得依靠基本的字节流对象进行操作

```java
public class Demo{
    public static void main(String[] args){
        //创建字节缓冲流输入对象
       BufferedInputStream bis = new BufferedInputStream(new FileInputStream("D:\\Demo\\java.png"));
        
        //创建字节缓冲流输出对象
        BufferOutputStream bos = new BufferedOutputStream(new FileOutputStream("D:\\Demo\\java1.png""));
                                                                               
        //读写操作
       byte[] bys = new byte[1024];
        int len;
        while((len = bis.read(bys))!= -1){
            bos.write(bys,0,len);
        }                                                                       
         //释放资源
         bis.close();                                                                     
         bos.close();                                                                      
                                                                               
    }
}
```

## 字符流

* 为什么出现字符流？
  * 字符流 = 字节流 + 编码表

* 用字节流复制文本文件时，文本文件也会有中文，但是没问题，原因是最终底层操作会自动进行字节拼接成中文，如何识别是中文的呢？
  * 汉字在进行存储的时候，无论选择哪种编码存储，第一个字节都是负数

###  IDEA平台下的字符串的编码解码问题

* 编码
  * byte[] getBytes()：使用平台默认的字符集将该String编码为一系列字节，将结果存储到新的字节数组中
  * byte[] getBytes(String charsetName)：使用指定的字符集将该String编码为一系列字节，将结果存储到新的字节数组中

* 解码
  * String(byte[] bytes)： 通过使用平台的默认字符集解码指定的字节数组来构造新的String
  * String(byte[] bytes,String charsetName)：通过指定的字符集解码指定的字节数组来构造新的String

### 字符流中的编码解码问题

* 字符流抽象基类
  * Reader：字符输入流的抽象类
  * Writer：字符输出流的抽象类

* 字符流中和编码解码问题相关的两个类：
  * InputStreamReader：是从字节流到字符流的桥梁
    * 它读取字节，并使用指定的编码将其解码为字符
    * 它使用的字符集可以由名称指定，也可以被明确指定，或者可以接受平台的默认字符集
  * OutputStreamWriter
    * 是从字符流到字节流的桥梁，使用指定的编码将写入的字符编码为字节
    * 它使用的字符集可以由名称指定，也可以被明确指定，或者可以接受平台的默认字符集

### 字符流写数据的5种方式

| 方法名                                     | 说明                                     |
| ------------------------------------------ | ---------------------------------------- |
| void write(int c)                          | 写一个字符                               |
| void write(char[] cbuf)                    | 写入一个字符数组                         |
| void write(char[]  cbuf, int off, int len) | 写入字符数组的一部分                     |
| void write(String str)                     | 写一个字符串                             |
| void write(String str,int off ,int len)    | 写一个字符串的一部分                     |
| voif flush()                               | 刷新流，还可以继续写数据                 |
| void close()                               | 关闭流，***先刷新***，关闭后不能再写数据 |

### 字符流读数据的2种方式

| 方法名                | 说明                   |
| --------------------- | ---------------------- |
| int read()            | 一次读一个字符数据     |
| int read(char[] cbuf) | 一次读一个字符数组数据 |

```java
//例：
public class Demo{
    public static void main(String[] args) throws IOException{
        InputStreamReader isr = new InputStreamReader(new FileInputStream("D:\\Demo\\java.txt"));
        
        OutputStreamWrite osw = new OutputStreamWrite(new FileOutputStream("D:\\Demo\\java.txt"));
        
        char[] chs = new char[1024];
        int len;
        while((len = isr.read(chs))!= -1){
            osw.write(chs,0,len);
        }
        
        osw.close();
        isr.close();
    }
}
```

```java
//更为简便的写法，使用字符流子类实现
public class Demo{
    public static void main(String[] args) throws IOException{
        FileReader fr = new FileReader("D:\\Demo\\java.txt");
        FileWriter fw = new FileWriter("D:\\Demo\\java1.txt");
        
        char[] chs = new char[1024];
        int len;
        while((len = fr.read(chs))!= -1){
            fw.write(chs,0,len);
        }
        
        fr.close();
        fw.close();
    }
}
```

### 字符缓冲流

* 字符缓冲流：
  * BufferedWriter：将文本写入字符输出流，缓冲字符，以提供单个字符，数组和字符串的高效写入，可以指定缓冲区大小，或者可以接受默认大小，默认值足够大，可用于大多数用途
  * BufferedReader：从字符输入流读取文本，缓冲字符，以提供字符，数组和行的高效读取，可以指定缓冲区大小，或者可以使用默认大小，默认值足够大，可用于大多数用途

* 构造方法：
  * BufferedWriter(Writer out)
  * BufferedReader(Reader in)

```java
public class Demo{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new FileReader("D:\\Demo\\java.txt"));
        BufferedWriter bw = new BufferedWriter(new FileWriter("D:\\Demo\\java1.txt")); 
        
        char[] chs = new char[1024];
        int len;
        while((len = br.read(chs))!=-1){
            bw.write(chs);
        }
        
        br.close();
        bw.close();
        
    }
}
```

#### 字符缓冲流特有功能

* BufferedWriter：
  * void newLine()：写一行行分隔符，行分隔符字符串由系统属性定义

* BufferedReader：
  * public String readLine()：读一行文字，结果包含行的内容的字符串，不包括任何行终止字符，如果流的结尾已经到达，则为null

```java
//字符缓冲流特有功能实现文件复制
public class Demo{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new FileReader("D:\\Demo\\java.txt"));
        BufferedWriter bw = new BufferedWriter(new FileWriter("D:\\Demo\\java1.txt")); 
        /*
        char[] chs = new char[1024];
        int len;
        while((len = br.read(chs))!=-1){
            bw.write(chs);
        }
        */
        
        String line;
        while((line = br.readLine())!=null){
            bw.writer(line);
            bw.newLine();
            bw.flush();
        }
        br.close();
        bw.close();
        
    }
}
```

## 案例

```java
//复制单级文件夹
public class Demo {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\虚幻引擎\\123");

//        String name = file.getName();

        File file1 = new File("D:\\虚幻引擎\\456");

//        System.out.println(file1.exists());

        if (!file1.exists()){
            file1.mkdir();
        }

        File[] files = file.listFiles();


            for (File f :files){
                String name1 = f.getName();
                File file2 = new File(file1, name1);
                copy(f,file2);
            }



    }

    private static void copy(File file,File file1) throws IOException {
        FileInputStream fis = new FileInputStream(file);

        FileOutputStream fos = new FileOutputStream(file1);
        byte[] bys = new byte[1024];
        int len;
        while ((len = fis.read(bys))!=-1){
            fos.write(bys);
        }

        fis.close();
        fos.close();
    }
}
```

```java
//复制多级文件夹
public class Demo {
    public static void main(String[] args) throws IOException {
        //复制多级文件夹
        File file = new File("D:\\虚幻引擎\\123");

//        String name = file.getName();

        File file1 = new File("D:\\虚幻引擎\\456");
        if (!file1.exists()){
            file1.mkdir();
        }

        copyFolder(file,file1);
    }

    private static void copyFolder(File sfile, File lfile) throws IOException {
        if (sfile.isDirectory()){
            String name = sfile.getName();
            File file = new File(lfile, name);
            if (!file.exists()){
                file.mkdir();
            }
            File[] files = sfile.listFiles();
            for (File f: files){
                copyFolder(f,file);
            }
        }else {
            File file = new File(lfile, sfile.getName());
            copy(sfile,file);
        }

    }

    private static void copy(File file, File file1) throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(file));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(file1));

        byte[] bys = new byte[1024];
        int len;
        while ((len = bis.read(bys))!=-1){
            bos.write(bys);
        }

        bis.close();
        bos.close();
    }
}
```

```java
//通用处理异常
public class Demo {

    //jdk7,自动帮我们关闭了资源
    private static void copy1(File file, File file1) {
        try(BufferedInputStream bis = new BufferedInputStream(new FileInputStream(file));
            BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(file1));) {
            byte[] bys = new byte[1024];
            int len;
            while ((len = bis.read(bys))!=-1){
                bos.write(bys);
            }
        }catch (IOException e){
            e.printStackTrace();
        }





    }
    //一般写法
    private static void copy(File file, File file1)  {
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;
        try {
            bis = new BufferedInputStream(new FileInputStream(file));
            bos = new BufferedOutputStream(new FileOutputStream(file1));
            byte[] bys = new byte[1024];
            int len;
            while (true){
                try {
                    if (!((len = bis.read(bys))!=-1)) break;
                } catch (IOException e) {
                    e.printStackTrace();
                }
                try {
                    bos.write(bys);
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }finally {
           if (bis != null){
               try {
                   bis.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
            if (bos != null){
                try {
                    bos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
```

***

## 特殊操作流

### 标准输入流

* System类中有两个静态的成员变量
  * public static final InputStream in：标准输入流。通常该流对应于键盘输入或由主机环境或用户指定的另一个输入源

* 自己实现键盘录入
  * BufferReader br = new BufferedReader(new InputStreamReader(System.in));

* 由于写起来比较麻烦，java提供了一个类实现键盘录入
  * Scanner sc = new Scanner(System.in);

### 标准输出流

* System类中有两个静态的成员变量
  * public static final PrintStream out ： 标准输出流。通常该流对应于显示输出或由主机环境或用户指定的另一个输出目标

* 输出语句的本质：是一个标准的输出流
  * PrintStream os = System.out;
  * PrintStream类有的方法，System.out都可以使用

### 打印流

* 打印流分类：
  * 字节打印流：PrintStream
  * 字符打印流：PrintStream

* 打印流的特点：
  * 只负责输出数据，不负责读取数据
  * 有自己特有的方法

* 字节打印流
  * PrintStream（String  fileName）：使用指定的文件名创建新的打印流
  * 使用继承父类的方法写数据，查看的时候会转码，使用自己特有方法写数据，查看的数据原样输出

```java
public class Demo{
    public static void main(String[] args){
        PrintStream ps = new PrintStream("D:\\Demo\\123.txt");
        
        ps.write(65);//写到文件中的结果是A
        
        ps.print(99);//写到文件中的结果就是99
    }
}
```

* 字符打印流PrintWrite的构造方法

| 方法名                                     | 说明                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| PrintWrite(String fileName)                | 使用指定的文件名创建一个新的PrintWriter,而不需要自动执行刷新 |
| PrintWriter(Writer out, boolean autoFlush) | 创建一个新的PrintWriter<br /><br />*  out：字符输出流<br /><br />* autoFlush：一个布尔值，如果为真，则Println，printf，或format方法将刷新输出缓存区 |

```java
public class Demo{
    public static void main(String[] args){
        //使用指定的文件名创建一个新的PrintWriter,而不需要自动执行刷新
        PrintWriter pw = new PrintWriter("D:\\Demo\\123.txt");
        
        pw.println("java");
        /*
           执行了两步：pw.write("java");
                     pw.write("\r\n");
        */
        pw.flush();//因为是字符流所以需要刷新一下，比较麻烦，下面构造方法自动刷新
        
        
        //PrintWriter(Writer out, boolean autoFlush)：创建一个新的PrintWriter
        PrintWriter pw1 = new PrintWriter(new FileWriter("D:\\Demo\\123.txt"),true);//为true才会自动刷新
        
        pw1.println("java");
        /*
           执行了三步：pw1.write("java");
                     pw1.write("\r\n");
                     pw1.flush();
        */
        
        pw.close();
        pw1.close();
    }
}
```

### 对象序列化流

* 对象序列化流：ObjectOutputStream
  * 将java对象的原始数据类型和图形写入OutputStream，可以使用ObjectInputStream读取（重构）对象，可以通过使用流的文件来实现对象的持久化存储，如果流是网络套接字流，则可以在另一个主机上或另一个进程中重构对象

* 构造方法：
  * ObjectOutputStream(OutputStream out)：创建一个写入指定的OutputStream的ObjectOutputStream

* 序列化对象的方法：
  * void writeObject(Object obj)：将指定的对象写入ObjectOutputStream

* 注意：
  * 一个对象要想被序列化，该对象所属的类必须实现Serializable接口
  * Serializable是一个标记接口，实现该接口，不需要重写任何方法

```java
public class Demo{
    public static void main(String[] args){
        //ObjectOutputStream(OutputStream out)：创建一个写入指定的OutputStream的ObjectOutputStream
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("D:\\Demo\\123.txt"));
        
        Student s = new Student("aa",11);
        
        //void writeObject(Object obj)：将指定的对象写入ObjectOutputStream
        oos.writeObject(s);
        
        //释放资源
        oos.close();
    }
}
```

#### 对象序列化流的一个小问题

* 用对象序列化流序列化一个对象后，假如我们修改了对象所属的类文件，读取数据会不会出问题呢？
  * 会出问题，抛出InvalidClassException异常

* 如果出问题，如何解决呢？
  * 给对象所属的类加一个serivalVersionUID
    * private static final long serivalVersionUID = 42L;

* 如果一个对象的某个成员变量不想被序列化，又该如何实现呢？
  * 给该成员变量加**transient**关键字修饰，该关键字标记的成员变量不参与序列化过程

### 对象的反序列化流

* 对象反序列化流：ObjectInputStream
  * ObjectInputStream反序列化先前使用ObjectOutputStream编写的原始数据对象

* 构造方法
  * ObjectInputStream(InputStream in)：创建从指定的InputStream读取的ObjectInputStream

* 反序列化对象的方法：
  * Object readObject()： 从ObjectInputStream读取一个对象

```java
public class Demo{
    public static void main(String[] args){
        //ObjectInputStream反序列化先前使用ObjectOutputStream编写的原始数据对象
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("D:\\Demo\\123.txt"));
        
        //ObjectInputStream(InputStream in)：创建从指定的InputStream读取的ObjectInputStream
        Object o = ois.readObject;
        
        //类型转换
        Student s = (Student)o;
        
        System.out.println(s.toString());
        
        ois.clost();
    }
}
```

### 为什么要做序列化？

* 在分布式系统中，此时需要把对象在网络上传输，就要把对象数据转换为二进制形式，需要共享数据的javaBean对象都要做序列化
* 服务器钝化：如果服务器发现某些对象好久都没有活动了，那么服务器就会把这些内存中的对象持久化在本地磁盘文件中（java对象转换为二进制文件）；如果服务器发现某些对象需要活动时，先去内存中寻找，找不到再去磁盘文件中反序列化我们的对象数据，恢复成java对象，这样能节省服务器内存

#### 什么时候对象需要被序列化

* 把对象保存到文件中（存储到物理介质）
* 对象需要在网络上传输

#### 实际应用场景

1. 在很多应用中，都需要对某些对象进行序列化，让它们离开内存空间，入住物理硬盘，以便长期保存，如Web服务器中的Session对象，当有10万用户并发访问，就可能出现10万个Session对象，内存可能吃不消，于是Web容器就会把一些Session先序列化到硬盘中，等到要用的时候，再把保存在硬盘的对象还原到内存中
2. 当两个进行在进行远程通信时，彼此可以发送各种类型数据，无论是何种类型数据，都会以二进制序列的形式在网络上传送，发送方需要把这个java对象转换为字符序列才能在网络上传输；接受方则需要把字节序列回复为java对象

### Properties

* Properties概述：
  * 是一个Map体系的集合类
  * Properties可以保存到流中或从流中加载

* Properties作为集合的特有方法

| 方法名                                      | 说明                                                         |
| ------------------------------------------- | ------------------------------------------------------------ |
| Object setProperty(String key,String value) | 设置集合的键和值，都是String类型，底层调用HashTable方法put   |
| String getProperty(String key)              | 使用此属性列表中指定的键搜索属性                             |
| Set<String> stringPropertyNames()           | 从该属性列表中返回一个不可修改的键集，其中键及其对应的值是字符串 |

* Properties和IO流相结合的方法

| 方法名                                       | 说明                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| void load(InputStream inStream)              | 从输入字节流读取属性列表（键和元素对）                       |
| void load(Reader reader)                     | 从输入字符流读取属性列表（键和元素对）                       |
| void store(OutputStream out,String comments) | 将此属性列表（键和元素对）写入此Properties表中，以适合于使用load(InputStream)方法的格式写入输出字节流 |
| void Store(Writer writer,String comments)    | 将此属性列表（键和元素对）写入此Properties表中，以适合使用load（Reader）方法的格式写入输出字符流 |

```java
public class Demo{
    public static void main(String[] args){
        //调用方法
        myStore();
        
        myLoad();
    }
    
    //此方法展示Properties的Load方法2
    public static void myLoad(){
        Properties ps = new Properties();
        FileReader fr = new FileReader("D:\\Demo\\123.txt");
        
        ps.load(fr);
        
        fr.close();
    }
    //此方法展示Properties的Store方法
    public static void myStore(){
        Properties ps = new Properties();
        
        FileWriter fw = new FileWriter("D:\\Demo\\123.txt");
        
        ps.setProperty("1","a");
         ps.setProperty("2","b");
         ps.setProperty("3","b");
        
        ps.Store(fw,null);
        
        fw.close();
    }
}
```

***

### 猜数字游戏（Properties版）

```java
/*
   需求：写一个程序实现猜数字小游戏，只能试玩3次，如果还想玩，提示：请充值
   
   思路：
   1.写一个游戏类，里面有猜数字小游戏
   2.写一个测试类，测试类中有main()方法，main()方法包含以下步骤
     ① 从文件中读取数据到Properties集合，用load()方法实现
                        文件存在：Game.txt
                        里面有一个数据值：count=0
     ② 通过Properties集合获取到玩游戏的次数
     ③ 判断次数是否达到三次
              * 如果到了，给出提示
              * 不过不到：接着还能玩，count次数+1，重新写回文件，用Properties的store()方法实现
*/


public class Demo {
    public static void main(String[] args) throws IOException {
        //读取数据

        Properties ps = new Properties();
        FileReader fr = new FileReader("D:\\Demo\\123.txt");//从文件获取次数count的值

        ps.load(fr);

        fr.close();

        String property = ps.getProperty("count");

//        System.out.println(property);

        int count = Integer.parseInt(property);

        if (count < 3) {
            GuessNumber.start();
            count++;
            ps.setProperty("count",String.valueOf(count));
            FileWriter fw = new FileWriter("D:\\Demo\\123.txt");
            ps.store(fw,null);
            fw.close();
        }else {
            System.out.println("请充值");
        }

    }


}

```

* 游戏类

```java
//猜数字游戏类
public class GuessNumber {
    private GuessNumber(){}
    
    public static void start(){
        Random random = new Random();

        int number = random.nextInt(100) + 1;

        while (true){
            Scanner sc = new Scanner(System.in);
            System.out.println("输入数字");
            int guessnum = sc.nextInt();

            if (number>guessnum){
                System.out.println("小了");
            }else if (number<guessnum){
                System.out.println("大了");
            }else {
                System.out.println("恭喜！");
                break;
            }
        }
    }
}
```

## RandomAccessFile的常见用法

1. 什么情况需要用到RandomAccessFile？

> 我们平常创建流对象关联文件，开始读文件或者写文件都是从头开始的，不能从中间开始，如果是开多线程下载一个文件，我们之前用的FileReader和FileWriter等待都是无法完成的，而RandomAccessFile就可以解决这个问题，因为它可以指定位置读，指定位置写的一个类，通常开发过程中，多用于多线程下载一个大文件

2. 构造器

|                   构造方法                    |                             描述                             |
| :-------------------------------------------: | :----------------------------------------------------------: |
|  `RandomAccessFile(File file, String mode)`   | 创建一个随机访问文件流，从[`File`](../../java/io/File.html)参数指定的文件读取，并可选地写入。 |
| `RandomAccessFile(String name,  String mode)` | 创建随机访问文件流，以从中指定名称的文件读取，并可选择写入文件 |

* 创建RandomAccessFile类实例需要指定一个mode参数，该参数指定RandomAccessFile的访问模式；
  * 其中参数 mode 的值可选 "r"：随机读，"rw"：既读又写

3. 成员方法

| Modifier and Type |        方法        |                             描述                             |
| :---------------: | :----------------: | :----------------------------------------------------------: |
|      `long`       | `getFilePointer()` |                    查找当前所处的文件位置                    |
|      `long`       |     `length()`     |                 判断文件尺寸（返回文件长度）                 |
|      `void`       |  `seek(long pos)`  | 设置文件指针偏移，从该文件的开头测量，发生下一次读取或写入。 |

```java
public class UsingRandomAccessFile1 {
    public static void main(String[] args) {


        RandomAccessFile  r = null;
        RandomAccessFile rw = null;
        try {
            r = new RandomAccessFile(new File("D:\\虚幻引擎\\1.jpg"), "r");
            rw = new RandomAccessFile(new File("D:\\虚幻引擎\\3.jpg"), "rw");
            byte[] bys = new byte[1024];
            int len;
            while ((len = r.read(bys))!=-1){
                rw.write(bys,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (r != null){

                try {
                    r.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (rw != null){

                try {
                    rw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}

```

* 如果RandomAccessFile作为输出流时，写出到的文件如果不存在，则在执行过程中自动创建
  * 如果写出到的文件存在，则会对原有文件内容进行覆盖，默认从头覆盖

### 案例：

1. 创建下载线程类

```java
public class DownLoadThread extends Thread {
​
    private long start;
    private File src;
    private long total;
    private File desc;
​
    /**
     * 
     * @param start
     *            开始下载的位置
     * @param src
     *            要下载的文件
     * @param desc
     *            要下载的目的地
     * @param total
     *            要下载的总量
     */
    public DownLoadThread(long start, File src, File desc, long total) {
        this.start = start;
        this.src = src;
        this.desc = desc;
        this.total = total;
    }
​
    @Override
    public void run() {
        try {
            // 创建输入流关联源,因为要指定位置读和写,所以我们需要用随机访问流
            RandomAccessFile src = new RandomAccessFile(this.src, "rw");
            RandomAccessFile desc = new RandomAccessFile(this.desc, "rw");
​
            // 源和目的都要从start开始
            src.seek(start);
            desc.seek(start);
            // 开始读写
            byte[] arr = new byte[1024];
            int len;
            long count = 0;
            while ((len = src.read(arr)) != -1) {
                //分三种情况
                if (len + count > total) {
                     //1.当读取的时候操作自己该线程的下载总量的时候,需要改变len
                    len = (int) (total - count);
                    desc.write(arr, 0, len);
                    //证明该线程下载任务已经完毕,结束读写操作
                    break;
                } else if (len + count < total) {
                    //2.证明还没有到下载总量,直接将内容写入
                    desc.write(arr, 0, len);
                    //并且使计数器任务累加
                    count += arr.length;
                } else {
                    //3.证明改好到下载总量
                    desc.write(arr, 0, len);
                    //结束读写
                    break;
                }
            }
            src.close();
            desc.close();
​
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

2. 主方法

```java
public class TestRandomAccess {
​
    public static void main(String[] args) {
        //关联源
        File src = new File("a.txt");
        //关联目的
        File desc = new File("b.txt");
​
        //获取源的总大小
        long length = src.length();
        // 开两条线程,并分配下载任务
        new DownLoadThread(0, src, desc, length / 2).start();
        new DownLoadThread(length / 2 , src, desc, length - (length / 2)).start();
    }
​
}
```

