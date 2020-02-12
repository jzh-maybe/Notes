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
- 在IDEA中，首先通过Build->Build Project
```
H:\workspace-IDEA\JavaEE\out\production\JavaBase>javap -verbose com.hugeyurt.jvm.bytecode.MyTest2
```
