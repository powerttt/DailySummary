## JVM加载class文件的过程

7个过程：

1. 加载class
2. class验证
3. 准备
4. 解析
5. 初始化
6. 使用
7. 卸载



### 1. 加载class

通过类的全限定名，获取class文件的二进制流，将静态文件结构转换为方法运行的结构，将class对象缓存在内存中。

### 2. 验证

验证的目的是为了保障JVM运行时的安全

1. 文件格式验证：验证是否符合JVM规范，如JDK版本的验证，常量是否支持等

2. 元数据验证：对字节码进行语义分析，如校验是否有继承父类，是否继承不可继承的类

3. 字节码验证：通过数据流和控制流，确保语义是否正确，主要是方法区，如方法类型转换是否正确，跳转指令是否正确

4. 符号验证：在解析过程中发生的动作，主要是确保解析的正常运行

### 3. 准备

将类中声明的静态属性在方法区设定默认值，在初始化阶段会初始化实例变量

### 4. 解析
将代码的符号引用转换为直接引用
### 5. 初始化
初始化的五中条件（对类的主动引用）
1. JVM初始化主类main方法
2. new getstatic putstatic incokestatic这几条指令
3. 初始化子类，父类没有被初始化
4. 使用Class.forName（String name）加载类时
5. 使用java.lang.reflect.*的方法对类进行反射调用时

### 6.  使用
### 7. 卸载



