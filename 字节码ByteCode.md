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
  