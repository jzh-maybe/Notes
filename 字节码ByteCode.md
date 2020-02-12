# 字节码 ByteCode
## 二、Class类文件的结构
以下面这段程序使用JDK1.8编译生成的Class文件进行分析。
```
package com.hugeyurt.jvm.bytecode;

public class MyTest2 {
    String str = "Welcome";
    private int x = 5;
    public static Integer in = 5;

    public static void main(String[] args) {
        MyTest2 myTest2 = new MyTest2();
        myTest2.setX(8);
        in = 20;
    }

    public void setX(int x) {
        this.x = x;
    }
}
```
### 1. 编译和反编译
- 在IDEA中，首先通过Build->Build Project，默认在Java项目根目录下编译生成.class文件；
- 在IDEA - Terminal中使用`javap`命令对.class文件进行反编译
  ```
  javap com.hugeyurt.jvm.bytecode.MyTest2
  ```
  使用`javap -c`命令可以打印出包括助记符在内的更为详细的信息
  ```
  javap -c com.hugeyurt.jvm.bytecode.MyTset2
  ```
  使用`javap -verbose'命令可以输出完整的字节码反编译信息
  ```
  H:\workspace-IDEA\JavaEE\out\production\JavaBase>javap -verbose com.hugeyurt.jvm.bytecode.MyTest2
  Classfile /H:/workspace-IDEA/JavaEE/out/production/JavaBase/com/hugeyurt/jvm/bytecode/MyTest2.class
    Last modified 2020-2-12; size 848 bytes
    MD5 checksum 3b69aa97d8201382d2b26561da60bd89
    Compiled from "MyTest2.java"
  public class com.hugeyurt.jvm.bytecode.MyTest2
    minor version: 0
    major version: 52
    flags: ACC_PUBLIC, ACC_SUPER
  Constant pool:
     #1 = Methodref          #10.#34        // java/lang/Object."<init>":()V
     #2 = String             #35            // Welcome
     #3 = Fieldref           #5.#36         // com/hugeyurt/jvm/bytecode/MyTest2.str:Ljava/lang/String;
     #4 = Fieldref           #5.#37         // com/hugeyurt/jvm/bytecode/MyTest2.x:I
     #5 = Class              #38            // com/hugeyurt/jvm/bytecode/MyTest2
     #6 = Methodref          #5.#34         // com/hugeyurt/jvm/bytecode/MyTest2."<init>":()V
     #7 = Methodref          #5.#39         // com/hugeyurt/jvm/bytecode/MyTest2.setX:(I)V
     #8 = Methodref          #40.#41        // java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
     #9 = Fieldref           #5.#42         // com/hugeyurt/jvm/bytecode/MyTest2.in:Ljava/lang/Integer;
    #10 = Class              #43            // java/lang/Object
    #11 = Utf8               str
    #12 = Utf8               Ljava/lang/String;
    #13 = Utf8               x
    #14 = Utf8               I
    #15 = Utf8               in
    #16 = Utf8               Ljava/lang/Integer;
    #17 = Utf8               <init>
    #18 = Utf8               ()V
    #19 = Utf8               Code
    #20 = Utf8               LineNumberTable
    #21 = Utf8               LocalVariableTable
    #22 = Utf8               this
  #23 = Utf8               Lcom/hugeyurt/jvm/bytecode/MyTest2;
  #24 = Utf8               main
  #25 = Utf8               ([Ljava/lang/String;)V
  #26 = Utf8               args
  #27 = Utf8               [Ljava/lang/String;
  #28 = Utf8               myTest2
  #29 = Utf8               setX
  #30 = Utf8               (I)V
  #31 = Utf8               <clinit>
  #32 = Utf8               SourceFile
  #33 = Utf8               MyTest2.java
  #34 = NameAndType        #17:#18        // "<init>":()V
  #35 = Utf8               Welcome
  #36 = NameAndType        #11:#12        // str:Ljava/lang/String;
  #37 = NameAndType        #13:#14        // x:I
  #38 = Utf8               com/hugeyurt/jvm/bytecode/MyTest2
  #39 = NameAndType        #29:#30        // setX:(I)V
  #40 = Class              #44            // java/lang/Integer
  #41 = NameAndType        #45:#46        // valueOf:(I)Ljava/lang/Integer;
  #42 = NameAndType        #15:#16        // in:Ljava/lang/Integer;
  #43 = Utf8               java/lang/Object
  #44 = Utf8               java/lang/Integer
  #45 = Utf8               valueOf
  #46 = Utf8               (I)Ljava/lang/Integer;
  ```
