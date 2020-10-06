# JVM

## 1. Java虚拟机是什么？

虚拟机是一种抽象化的计算机，通过在实际的计算机上仿真模拟各种计算机功能来实现的。Java虚拟机有自己完善的硬体架构，如处理器、堆栈、寄存器等，还具有相应的指令系统。Java虚拟机屏蔽了与具体操作系统平台相关的信息，使得Java程序只需生成在Java虚拟机上运行的目标代码（字节码），就可以在多种平台上不加修改地运行。Java语言的一个非常重要的特点就是与平台的无关性。而使用Java虚拟机是实现这一特点的关键。一般的高级语言如果要在不同的平台上运行，至少需要编译成不同的目标代码。而引入Java语言虚拟机后，Java语言在不同平台上运行时不需要重新编译。Java语言使用模式Java虚拟机屏蔽了与具体平台相关的信息，使得Java语言编译程序只需生成在Java虚拟机上运行的目标代码（字节码），就可以在多种平台上不加修改地运行。Java虚拟机在执行字节码时，把字节码解释成具体平台上的机器指令执行。

## 2. 机器码和字节码

**机器码**：

计算机直接使用的程序语言，其语句就是机器指令码，机器指令码是用于指挥计算机应做的操作和操作数地址的一组二进制数

**字节码**：

Java之所以可以“一次编译，到处运行”，一是因为JVM针对各种操作系统、平台都进行了定制，二是因为无论在什么平台，都可以编译生成固定格式的字节码（.class文件）供JVM使用。因此，也可以看出字节码对于Java生态的重要性。之所以被称之为字节码，是因为字节码文件由十六进制值组成，而JVM以两个十六进制值为一组，即以字节为单位进行读取

## 3. JVM运行时数据分区

1.线程共享区：堆、方法区

2.线程独占区：程序计数器、虚拟机栈、本地方法栈

3：**程序计数器**：当前线程所执行的Java字节码的行号   

**虚拟机栈**：每个方法在执行的同时都会创建一个栈帧（Stack Frame）用于存储局部变量表（ 存放了各种基本数据类型 、对象引用）、操作数栈、动态链接、方法出口。如果线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常。扩展时无法申请到足够的内存，就会抛出OutOfMemoryError异常。   **堆**：主要存放对象实例，所以Java堆中还可以细分为：新生代和老年代；新生代可以有Eden空间、From Survivor空间、To Survivor空间   

**方法区**：存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据，即存放静态文件，如Java类、方法等   

1.6 运行时常量池放在方法区中，1.7在堆中，1.8在元空间中

## 4. JVM类加载机制

**1.加载** 通过类的全限定名获得这个类的二进制字节流，把二进制字节流按照方法区的数据存储结构存储在方法区中，在内存中生成一个代表这个类的class对象，作为方法区这个类的数据访问入口

**2.验证** 文件格式验证，验证二进制字节流是否符合class文件格式规范，如是否以魔数cafebaby开头，元数据验证，验证字节码描述的语义是否符合Java语言规范，如这个类是否实现了接口中的所有方法，字节码验证，验证程序语义是符合规范的，符号引用验证

**3.准备** 为类变量（static修饰的变量）设置初始值0，但是实例变量不会设，会跟随对象一起进入堆中。Final修饰的会被直接设置所定义的值

**4.解析** 把常量池中的符号引用（符号引用引用的目标并不一定已经加载到内存中）替换为直接引用（直接引用可以是直接指向目标的指针、相对偏移量，直接引用引用的目标必定已经在内存中存在）

**5初始化** 初始化阶段是执行类构造器<clinit>（）方法的过程，类的构造方法用于对类的成员变量进行初始化，程序是默认添加一个无参构造的，如果有需要可以重载这个方法，可以在创建对象的时候传入参数对对象初始化。子类继承父类，子类必须在自己的构造方法第一行用super()显式的调用父类的构造方法，如果父类只有无参构造方法，且不打算重写子类的构造方法，为节省代码量，子类构造方法可以不写，但是实际上是已经写了，系统默认会在子类的无参构造方法中的第一行加上父类的无参构造方法