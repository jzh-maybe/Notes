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
### 2. 使用十六进制编辑器WinHex打开此Class文件
![批注 20200212 140419.jpg](0)
### 3. 字节码分析
#### （1）魔数 magic
每个Class文件的头4个字节称为魔数(Magic Number)，它的唯一作用是确定这个文件是否为一个能被虚拟机接受的Class文件，值为0xCAFEBABE。

#### （2）版本号 minor_version&major_version
紧接着魔数的4个字节存储的是Class文件的版本号，第5和第6个字节是次版本号(minor_version)，第7和第8个字节为主版本号(major_version)。因此这里的值`00 00 00 34`对应的次版本号为0，主版本号为52（即JDK1.8）。说明该文件是可以被JDK1.8或以上版本虚拟机执行的Class文件。
#### （3）常量池 constant pool
- 主次版本号之后的是常量池入口，常量池可以理解为Class文件中的资源仓库。
**常量池中主要存放两大类常量：字面量和符号引用**。字面量包括文本字符串和声明为final的常量值等，符号引用包括类或接口的全限定名、字段的名称和描述符以及方法的名称和描述符等。
- 常量池结构由常量池容量计数值（u2类型）和常量池数组组成。**常量池数组中的元素个数=常量池容量计数值-1，索引为0的常量不位于常量池数组中，常量池数组的索引从1开始。** 常量池数组中的每一项常量都是一个表结构，所有表开始的第一位都是一个u1类型的标志位tag，代表该常量所属的常量类型。
![59513720181219204338051305022474.png](1)
- 常量池字节码具体分析
`00 2F`对应的十进制为15+2*16=47，即常量池数组中有46个元素。
	- 常量1：`0A`对应十进制的10，查表得出对应的常量类型为`CONSTANT_Methodref_info`，类中方法的符号引用；接下来2个字节`00 0A`对应的十进制数值为10，表示声明此方法的类或者接口描述符的索引项为常量池数组中的第10个常量；再向后2个字节`00 22`对应十进制数34，表示指向此方法的名称及类型描述符的索引项为34。
	- 常量2：`08`对应十进制的8，对应的常量类型为`CONSTANT_String_info`，字符串类型的字面量；接下来2个字节`00 23`对应的十进制数值为35，表示指向字符串字面量的索引为35。
	- 常量3：`09`对应十进制的9，对应的常量类型为`CONSTANT_Fieldref_info`，字段的符号引用；接下来2个字节`00 05`对应的十进制数值为5，表示声明该字段的类或接口描述符的索引项为5；再往后2个字节`00 24`对应的十进制数值为36，表示指向此字段的名称和类型描述符的索引项为36。
	- 常量4：`09`对应十进制的9，对应的常量类型为`CONSTANT_Fieldref_info`，字段的符号引用；接下来2个字节`00 05`对应的十进制数值为5，表示声明该字段的类或接口描述符的索引项为5；再往后2个字节`00 25`对应的十进制数值为37，表示指向此字段的名称和类型描述付的索引项为37。
	- 常量5：`07`对应十进制的7，对应的常量类型为`CONSTANT_Class_info`，类或接口的符号引用；接下来2个字节`00 26`对应的十进制数值为38，表示指向全限定名常量项的索引为38。
	- 常量6：`0A`对应十进制的10，对应的常量类型为`CONSTANT_Methofref_info`，类中方法的符号引用；接下来2个字节`00 05`对应的十进制数值为5，表示声明此方法的类或者接口描述符的索引项为5；再向后2个字节`00 22`对应的十进制数值为34，表示指向此方法的名称和类型描述符的索引项为22。