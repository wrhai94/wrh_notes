# 面向对象编程（高级）
## 类变量和类方法（静态）
1. static 变量是同一个类所有对象共享的。
2. static 类变量，在类加载的时候生成（所以不需要创建对象）。
### 静态变量的初始化
- 静态变量会按照声明的顺序先依次声明并设置为该类型的默认值，但不赋值为初始化的值。
- 声明完毕后,再按声明的顺序依次设置为初始化的值，如果没有初始化的值就跳过。

### 注意事项和细节
1. 静态方法可以通过对象名和类名访问。
2. 静态方法只能访问类中的静态变量和静态方法。

## main 方法
1. main 方法是虚拟机调用的。
2. 虚拟机需要调用类 main 方法，所以该方法的访问权限必须是public。
3. 虚拟机调用 main 方法不需要创建对象，所以必须是 static。
4. main 方法接收 String 类型的数组参数，该数组中保存执行 java 命令时传递给所运行的类的参数。
5. Java 执行程序时可以传参给这个数组。

### 给 main 传参
- 命令行，java 类名 参数1，参数2...
- IDEA，在右上角菜单运行按钮左边的类名下拉框，点击 Edit Configurations，在 Program arguments 填写参数。

## 代码块
- [修饰符]{代码};
  1) 修饰符只能写 static
  2) static 区分静态代码块和普通代码块
  3) ; 可以不写。
- 多个构造器中，出现重复代码，可以提取到代码块中。
- 在构建对象时，会先于构造器调用代码块。

### 注意事项和细节
1. 静态代码块，作用是对类的初始化，它随类的加载而执行，并且只会执行一次。普通代码块会在每创建一个对象时执行。
2. **类什么时候加载**
   1. 创建对象实例时
   2. 创建子类对象实例，父类也会被加载（父类先加载）
   3. 使用类的静态成员时
3. 普通代码块，在创建对象实例时，会被隐式的调用。被创建一次就会调用一次。
   如果只是调用类的静态成员时，普通代码块不会被执行。
3. **创建一个对象时，在一个类的调用顺序**
   1. 调用静态代码块和静态属性初始化（这个两个的优先级一样，按定义顺序调用）
   2. 调用普通代码块和普通属性的初始化（优先级一样，按定义顺序调用）
   3. 调用构造方法。
4. 构造器最前面隐含了 super() 和调用普通代码块。
   1. 父类普通代码块
   2. 父类构造器
   3. 本类普通代码块
   4. 本类构造器
5. 静态代码块，只能调用静态成员。普通代码块可以使用任意成员。

## 单例设计模式
1. 采取一定的方法保证在整个的软件系统中，对某个类只能存在一个对象实例，并且该类只提供一个取得其对象实例的方法。一般用于重量级的类。
2. 饿汉式、懒汉式。
   1. 构造器私有化。
   2. 在类的内部直接创建对象。
   3. 提供一个公共的 static 方法，返回类的实例对象。
```java
// 饿汉式
// 缺点：可能加载了类，但没有使用对象，会造成浪费
class A {
    private A(){}
    private static a = new A();
    public static A getInstance(){return a;}
}
```
```java
// 懒汉式
// 缺点：存在线程安全问题
class A {
    private A(){}
    private static a;
    public static A getInstance(){
        if(a == null) {
            a = new A();
        }
        return a;
    }
}
```

## final
1. final 修饰的类不能被继承
2. final 修饰的方法不能被子类重写
3. final 修饰的属性不能给修改
4. final 修饰的局部变量不能被修改

### 注意事项和细节
1. final 修饰的属性叫常量，命名格式：XX_XX_XX。
2. 常量必须赋初始值。
   1. 定义时。
   2. 构造器（每个构造器都需要赋值）。
   3. 代码块。
3. 如果常量是静态的。
   1. 定义时。
   2. 静态代码块。
4. final 不能修饰构造器。
5. final 和 static 搭配使用效率更高，调用静态常量不会导致类加载（不会执行静态代码块），编译器底层做了优化。
6. 包装类、String 被 final 修饰，不能被继承。

## 抽象类
### 注意事项和细节
1. 抽象类不能被实例化
2. 抽象类不一定包含抽象方法
3. 一旦类包含了抽象方法，这个类必须声明为抽象类
4. abstract 只能修饰类和方法。
5. 抽象类本质还是类，可以有类中的各种成员
6. 抽象方法不能有方法体
7. 一个类继承了抽象类，则它必须实现抽象类的所有抽象方法，除非它也声明为抽象类（即非抽象类中不能有抽象方法，包括父类继承过来的）。
8. 抽象方法不能使用 private、final 和 static 来修饰，因为这些关键字和重写相违背。

## 抽象类实践 - 模板设计模式

## 接口
### 注意事项和细节
1. 接口不能被实例化
2. 接口中的方法都是 public 方法
3. 实现类必须实现接口的所有方法
4. 抽象类可以不用实现接口的抽象方法
5. 一个类同时可以实现多个接口
6. 接口中的属性只能是 final,而且是 public static final
   int n = 1;   // 等价  public static final int n = 1;
7. 接口不能继承其他的类，但可以继承过个别的接口（extends）。
8. 访问级别只能是 public 和默认。

### 接口和继承
- 解决问题
  - 继承主要是解决代码的复用性和可维护性
  - 接口主要是设计好各种规范，让其他类去实现这些方法。
- 接口比继承灵活
- 接口在一定程度上实现代码解耦。

### 接口的多态特性
1. 多态参数，public void a (interface i){}
2. 多态数组
3. 接口存在多态传递现象。
4. 父类和接口出现同名属性，不能直接使用属性，父类的属性用 super 使用，接口的属性用 接口名.属性。

## 内部类
- 类的五大成员：属性、方法、构造器、代码块、内部类。
### 分类
- 定义在外部类局部位置
  1. 局部内部类
  2. 匿名内部类
- 定义在外部类的成员位置
  1. 成员内部类
  2. 静态内部类
### 局部内部类
1. 可以直接访问外部类的所有成员。
2. 不能添加访问修饰符，可以使用final.
3. 作用域：仅在定义它的方法或代码块中。
4. 外部类在方法中，可以创建内部类的对象。
5. 内部类和外部类成员重名，默认遵循就近原则，访问外部类成员使用 外部类名.this.成员名。

### ***匿名内部类（Anonymous Inner Class）***
1. 用于简化开发，当要创建一个只会使用一次的类，可以使用匿名内部类
2. jdk 底层在创建匿名内部类 外部类$1 后，立即就创建 外部类$1 的实例，并返回地址。

### 成员内部类

### 静态内部类
- - -

# 枚举和注解
## 自定义类实现枚举
## enum 关键字实现枚举
1. 使用 enum 替换 class；
2. 常量名(实参列表)，如 SPTING("春天","温暖")；
3. 如果存在多个常量，用 ',' 间隔；
4. 常量对象写在最前面。

###  Enum 常用方法
- name():输出对象的名字
- ordinal:输出枚举对象的序号
- values:返回所有枚举对象
- valueOf:将字符串转换成枚举对象，要求字符串必须为已有常量名，否则会报错
- compareTo:比较两个枚举常量，比较的是序号。

### 注意
- 枚举有一个隐式继承 Enum，所以 enum 修饰了就不能再继承其他类，可以实现接口。
- - -

## JDK 内置的基本注解类型
- @override
  - 表示指定重写的方法，如果父类没有改方法，会报错。
  - 只能修饰方法。
  - 查看注解源码为 @Target(ElementType.METHOD)，说明只能修饰方法。
  - @Target 为修饰注解的注解，称为元注解。
- @Deprecated
  - 修饰某个元素，表示该元素已经过时了。
- @SuppressWarnings
  - 抑制警告信息
  - 可以在 {""} 中写入抑制警告的类型，@SuppressWarnings({"all"})
## 元注解：对注解进行注解（了解即可，用于读原码）
- - -

# 异常
## 异常的概念
## 异常体系图*
## 常见异常
### NullPointerException
### ArithmeticException
### ArrayIndexOutOfBoundsException
### ClassCastException
### NumberFormatException

## 异常处理概念
### 异常处理方式
#### try-catch-finally
- 捕获异常，自行处理
- 注意 
  - 可以用多个 catch 分别捕获不同的异常，做相应的处理，要求子类异常写在父类异常前面。
#### throws
- 抛出异常，最高级处理者是JVM
- 注意
  - 对于编译异常，程序中必须处理
  - 对于运行时异常，程序中如果没有处理，默认就是 throws 的方式处理。（java 不要求程序员处理运行时异常，因为有默认处理方式）
  - 子类重写父类的方法时，对抛出异常的规定：子类重写的方法，所抛出的异常不能缩小父类方法的异常（一致或是其子类型）。
## 异常处理分类
## 自定义异常
- 一般自定义异常是继承 RunTimeException，好处是可以使用默认处理方式（不用在编译时处理异常）。
## throw 和 throws 的对比
- - -

# 常用类
## 包装类
1. 针对八字基本数据类型相应的引用类型。
2. 有了类的特点，可以调用类中的方法。
3. JDK 5 前是手动装箱，之后是自动装箱。
4. 自动装箱底层还是用了 valueOf。
> 两个数值相等的 Integer 用 == 比较时，如果是 new 出来的，返回 false，如果是直接赋数值，会调用 valueOf 自动装箱，valueOf 中，-128 ~ 127 会从缓存中读取，所以返回 true，不在这个范围的需要 new 一个新的对象，所以返回 false。
> 如果基础数据类型和包装类类型用 == 比较，比较的是数值
## String*
1. String 对象用于保存一组字符序列。
2. 使用 Unicode 字符编码，一个字符（不区分字母汉字）占两个字节。
3. 属性 private final char value[]，用于存放字符串内容（final 修饰，说明 value 不能指向一个新的地址）。
4. String 被 final 修饰，无法被继承。

### String 创建方式
- 直接赋值 String s = "s"：先从常量池找是否有相同字符串的数据空间，如果有，直接指向；如果没有则重新创建。s 最终指向常量池的空间地址。
- 调用构造器 String s2 = new String("s")：先在堆中创建空间，value 属性指向常量池对应字符串空间。如果常量池没有对应值，重新创建，如果又直接指向。最终指向的是堆中的空间地址。
- ![String创建机制](F:/note/file/java/MyJava/img/String创建机制.png)

### String 特性
- String a = "he" + "llo"; 编译器会优化为 String a = "hello"; 这个过程只创建了一个对象。
- String a = "he";
  String b = "llo";
  String c = a + b;
  创建一个 StringBuffer,调用 append 分别拼接 a 和 b ，最后 toString。

## StringBuffer*
1. 继承于父类 AbstractStringBuilder,父类有属性 char[] value,不是 final，所以存放在堆中。
2. 被 final 修饰，不能被继承。
3. 在变化字符时，不用每次都创建新的对象，效率高于 String。

### 构造器的使用
1. 无参构造器，创建一个大小为 16 的 char[]。
2. new StringBuffer(100),指定 char[] 大小。
3. new StringBuffer("hello"),参数列表的字符串长度 + 16。

### 注意
- append(null),原码会调用一个 appendNull 方法，在value 中添加 'n','u','l','l'。
- new StringBuffer(null),会执行 super(str.length()+16),所以会报错。
## StringBuilder*
- 用在字符串缓冲区被单个线程使用，在运行速度上StringBuffer因为兼顾了线程安全，效率不及StringBuilder，建议在单线程时，优先使用 StringBuilder。
StringBuffer是线程安全的。

### String 、 StringBuffer 和 StringBuilder
- 使用原则
  - 字符串存在大量修改，使用 StringBuffer 或 StringBuilder。
  - 字符串存在大量修改，并在单线程的情况下，使用 StringBuilder。
  - 字符串存在大量修改，并在多线程的情况下，使用 StringBuilder。
  - 字符串很少修改，被多个对象引用，使用 String。

## Math
## Date、Calender、LocalDate...
### Calender
- 存在的问题
  - 可变性，像日期和时间应该是不可变的
  - 偏移性，Date 中的年份是从 1900 年开始的，而月份是从 0 开始的
  - 格式化，不能像 Date 一样进行格式化。
  - 不是线程安全，不能处理闰秒（2天多出1s）

### LocalDate
- LocalDate(日期)，LocalTime(时分秒)，LocalDateTime(年月日 时分秒)
- JDK8 后加入。
- DateTimeFormatter , 格式化。
- instant ,时间戳

## System
- exit
- arraycopy
- currentTimeMillis
## Arrays
## BigInteger BigDecimal
- - -

# 集合
## 集合框架体系
1. 单列集合
![单列集合](F:/note/file/java/MyJava/img/单列集合.png)
2. 双列集合（K-V）
![双列集合](F:/note/file/java/MyJava/img/双列集合.png)
## Collection 接口
### 常用方法
- add，基本类型会被自动装箱
- remove,参数可以是下标，也可以是指定元素的值
- contains
- size
- clear
- addAll
- containsAll,参数是集合，查找多个元素是否都存在。
- removeAll
  
### 遍历
- 迭代器
  - Collection 接口继承了 Iterable 接口
  - next,hasNext 遍历（Idea 输入 itit 可以快速生成while 语句）。
  - 循环结束后，迭代器指向最后一个元素，如果想再次遍历，需要再次调用 iterator 重置迭代器。
- 增强 for 循环，底层也是使用迭代器，可以理解为简化版的迭代器遍历(idea 输入 I 快速生成代码). 

### List 接口
- Collection 的子接口
- 元素有序，可重复
- 每个元素有对应的顺序索引

#### 常用方法
- add,不带索引，默认插入到最后，带索引插入到指定位置， add(1,"aa")。
- addAll,同样可以带索引
- indexOf,返回原始第一次出现的位置
- lastIndexOf,最后一次出现的位置
- remove
- set
- subList(int fromIndex, int toIndex),返回[fromIndex, toIndex) 的集合

#### ArrayList
- 可以加入 null 元素
-  Vector 基本相同，ArrayList 是线程不安全（没有 synchronized 效率高），多线程情况下不建议使用。
- 底层由数组实现数据存储,Object 类型的数据 elementData， tansient Object[] elementData; // tansient 表示该属性不会被序列化。
- 无参构造器初始的 elementData ，大小为0，第一次添加元素，扩容10，后续按 1.5 倍扩容
- 指定大小构造器，elementData 容量为字段大小。

#### Vertor
- 无参构造是，默认大小是 10 ， 按 2 倍（可自定义扩容大小）扩容。

#### LinkedList
- 底层实现了双向链表和双端队列的特点，不是数组，不涉及到扩容，效率较高。
- 可以添加容易元素，包括 null
- 线程不安全，没实现同步。

### Set 接口
- 无序，没有索引，所以不能使用索引的方式遍历。
- 不允许重复元素，可以添加 null 。
- 添加和取出的顺序不一致，但取出的顺序是不变的。
- 可以使用迭代器和增强 for 循环遍历。

#### HashSet
- 底层是 HashMap
- 不保证元素有序

##### 底层机制
1. 添加一个元素时，先得到 hash 值，再转换为索引值。
2. 找到存储数据表 table，看索引位置是否已存在元素。
3. 如果没有，增加加入
4. 如果有，调用 equals 比较，相同就放弃添加，不相同加到链表最后。
5. Java 8 中，如果一条列表元素达到 TREEIFY_CAPACITY(默认 8)，并且 table 的大小 >= MIN_TREEIFY_CAPACTIY(默认 64)，就会进行树化（红黑树）。

##### 扩容和转成红黑树机制
1. 第一次添加时， table 数组扩容到 16，临界值 threshold 是 16 * 加载因子（0.75） = 12。
2. 如果 table 数组使用到了临界值 12 ，就扩容到 16 * 2 = 32，新的临界值就是 32 * 0.75 = 24，以此类推。
3. Java 8 中，如果一条列表元素达到 TREEIFY_CAPACITY(默认 8)，会加入树化的方法，树化方法中判断 table 的大小 >= MIN_TREEIFY_CAPACTIY(默认 64)，就会进行树化（红黑树），否则进行树化。

#### LinkedHashSet
- 是 HashSet 的子类
- 根据元素的 hashCode 值决定元素的存储位置，同时使用链表维护元素的次序，使得元素看起来是以插入顺序保存的
- 不允许添加重复元素。
- 底层维护了一个数组 + 双向链表（LinkedHashMap,HashMap 的子类）
- 数组是 HashMap$Node[], 存放的元素是 LinkedHashMap$Entry 类型
- LinkedHashMap$Entry 类型的元素有 befor 和 after 属性，可以确定元素的顺序。

## Map
- 用于保存具有映射关系的数据。
- Map 中的 key 和 value 可以是任何引用类型的数据，会封装到  HashMap$Node 对象中。
- key 不可以重复，value 可以重复。
- key 和 value 都可以为空。
- k-v 存放在 HashMap$Node
- 为了方便遍历 还创建了一个 EntrySet 集合，该集合存放 Entry 类型的元素，Entry 中有 k,v ， EntrySet\<Entery\<K,V\>\>
- entrySet 中，定义类型是 Map.Entry,实际类型是 HashMap$Node
- 线程不安全

### HashMap

### HashTable
- 存放键值对
- 键和值不能为 null , 否则抛出异常
- 使用方法基本和 HashMap 一样
- 线程安全

#### 底层分析
- 底层有数组 Hashtable$Entry[] ,初始大小 11
- 临界值 大小 * 0.75
- 达到临界值扩容，原大小 << 1 + 1。

#### Properties
- 继承于 Hashtable 并实现了 Map 接口，也是保存键值。
- 使用和 Hashtable 相似。
- **可以用于从 XXX.properties 文件中**，加载数据到 Properties 类对象，并进行读取修改。
- IO 流读取。

### 遍历方式
1. 增强 for 遍历 keySet,for(Object key : map.keySet()){mage.get(key);}
2. 迭代器遍历 keySet， map.keySet().iterator()
3. 增强 for 遍历 values, for(Object value : map.values()){}
4. 迭代器遍历 values
5. 增强 for 遍历 entrySet
6. 迭代器遍历 entrySet

![集合使用选择](F:/note/file/java/MyJava/img/集合使用选择.png)


## TreeSet 和 TreeMap
- TreeSet 底层用 TreeMap 存储
- 可以用匿名内部类传一个比较器进去
- 如果没有传比较器，使用的是传入的对象实现的 Comparable 接口的comparaTo（会将传入的对象转为 Comparable 类型，所以没有传比较器时，传入的对象必须实现 Comparable 接口）。
- 比较器返回 0 ，认为是重复元素，不添加（去重机制）
- 根据比较器的规则决定 key 的排序。
- TreeSet 的值存放在 TreeMap 的 key 上。

## Collections 工具类
- reverse ： 反转 List 中的元素。
- shuffle : 对 List 的元素进行随机排序
- sort : 默认自然排序，可以传比较器
- swap(List list, int i, int j) : 交换 i 和 j 的位置
- max : 自然顺序的最大值，可以传比较器
- frequency ： 返回集合中指定元素出现的次数。
- copy(List dest, List src) : 复制集合，dest 大小至少需要和 src 一样。
- replaceAll(list, obj1, obj2) : 如果 List 中有 obj1 就替换成 obj2。

# 泛型
- 泛型推荐使用形式,缩略等号右边的类型变量，ArrayList\<String\> list = new ArrayList<>()
- 不指定类型变量，默认是 Object
- 类型变量是在对象创建的时候指定的
- 类型变量不能初始化
- 静态方法不能使用类型变量

## 泛型接口
- 接口中，静态成员不能使用泛型
- 泛型接口的类型，在**继承或实现**接口是确定
- 没有指定类型3，默认 Object

## 泛型方法
- 泛型方法可以定义在普通类中，也可以定义在泛型类型中
- 当泛型方法被调用时，泛型会确定

## 泛型的继承和通配
- List<?>：可以传任意类型
- List<? extends AA>:可以接受 AA 或 AA 的子类
- List<? super AA>:可以接受 AA 或 AA 的父类


# Java 事件处理机制
- java 事件处理是采取“委派事件模型”。当事件发生时，产生事件的对象，会把此消息传递给事件监听者，这里消息指的是 java.awt.event 事件类库里某个类所创建的对象，把它称为“事件对象”。

# 线程
- 程序：为完成特定任务、用某种语言编写的一组指令的集合。
- 进程
  - 指运行中的程序，启动一个进程，操作系统就会为改进程分配内存空间。
  - 进程是程序的一次执行过程，或是正在运行的一个程序，是多态过程，有产生、存在和消亡的过程。
- 线程
  - 线程是由进程创建的，是进程的一个实体
  - 一个进程可以拥有多个线程
- 单线程：同一个时刻，只允许执行一个线程
- 多线程：同一个时刻，可以执行多个线程
- 并发：同一个时刻，多个任务交替执行，造成一种“同时”的错觉，简单说，单核 CPU 实现的多个任务就是并发。
- 并行同一个时刻，多个任务同时执行。多核 CPU 可以实现并行。
- 可以在控制台用 jconsole 查看线程。
## 线程的使用
### 继承 Thread 实现
1. 一个类继承 Thread ，该类就可以当做一个线程使用
2. 重写 run 方法， Thread 类实现了 Runable 接口的 run 方法。
3. 创建线程类对象
4. 调用线程类对象的 start 方法开启线程，该线程和原来调用start 的线程是并发或并行执行的。如果是直接调用 run 方法，仅仅只是执行 run 方法，没有开启新的线程，原来的线程会阻塞，直到执行完 run 方法才能继续执行。

### 实现 Runable 接口 （静态代理）
1. 一个类实现 Runable 接口
2. 实现 run 方法
3. 创建 Thread 对象，把实现 Runable 接口的类的对象传给 Thread 的构造器。
4. Thread 对象调用 start 方法。
5. 可以将一个实现 Runable 接口的类的对象，分别传给两个 Thread 对象，因为线程是在调用 start 方法时创建的，所以可以实现多个线程共享一个 Runable 实现对象
6. 建议使用这种方式

### Thread 机制
- Thread 中有一个 Runable target 对象
- 通过构造器传一个实现 Runable 接口的类对象给 target
- run 方法中，如果 target != null,执行 target.run();
- Thread 的 start 方法被调用时，会调用 start0 方法（由虚拟机调用），start0 会执行 run 方法。

### 其他
- 售票问题，多线售票，可能会超出票数上限
- 可以给线程设置一个控制变量 boolean loop = true,当外界可以修改控制变量，控制线程是否结束。
## 线程方法
- start : 底层会创建一个新的线程。
- setPriority : 线程优先级(Thread.MIN_PRIORITY)
- interrupt : 中断线程，没有真正结束线程，一般用于中断正在休眠的线程
- sleep : 休眠线程
- Thread.currentThread().getName() 获取当前线程名称

### 线程让步和插队
- yield : 线程让步，让出 CPU ，让其他线程执行，这种让步的时间不确定，所以不一定让步成功（CPU 资源越充分，让步越可能不成功，因为 CPU 觉得资源充足，不需要让步）。
- join ： 线程插队，插队的线程，插队的线程一旦插队成功，肯定先执行完插入的线程的所有任务。

### 守护线程和用户线程
1. 用户线程：也叫工作线程，当线程的任务执行完或通知方式结束
2. 守护线程：一般为工作线程服务的，当所有的用户线程结束，守护线程自动结束。
3. 常见的守护线程：垃圾回收机制。

#### 守护线程的使用
- 当希望主线程结束，子线程也结束时，可以将子线程设置为守护线程
- 使用：子线程对象.setDaemon(true); 子线程对象.start(); 
## 线程生命周期
![线程的生命周期](F:/note/file/java/MyJava/img/线程的生命周期.jpg)
## Synchronized
1. 在多线程编程，一些敏感数据不允许被多个线程同时访问，此时就使用同步访问技术，保证数据再任何同一时刻，最多有一个线程访问，已保证数据的完整性。
2. 也可以理解：线程同步，即当一个线程在堆内存进行操作时，其他线程都不可以对这个内存地址进行操作，直到这个线程完成操作。

### 同步具体方法
1. 同步代码块
   synchronized (对象名|静态方法用当前类.class) {// 得到对象的锁，才能操作同步代码}
2. synchronized 还可以放在方法声明中，表示整个方法为同步方法
   public synchronized void method(){}

## 释放锁
![释放锁](F:/note/file/java/MyJava/img/释放锁.png)
![释放锁的分析](F:/note/file/java/MyJava/img/释放锁的分析.png)
## 互斥
1. 用于保证共享数据操作的完整性
2. ，每个对象都对应一个可称为 “互斥锁” 的标记，当某个对象用 synchronized 修饰时，表明该对象在任一时刻只能由一个线程访问。
3. 同步局限性：降低程序执行效率
4. 非静态同步方法的锁可以是 this, 也可以是其他对象（要求是同一个对象）
5. 静态同步方法的锁为当前类的本身。
## 死锁

# IO 流
## 文件
### 概念
- 文件就是保存数据的地方
- 文件流：文件在程序中是以流的形式来操作的
  - 流：数据再数据源（文件）和程序（内存）之间经历的路径
  - 输入流：数据从数据源到程序的路径
  - 输出流：数据从程序到数据源的路径
### 常用操作
- 创建文件对象性相关构造器和方法
  - new File(String pathname) // 根据路径构建 File 对象
  - new File(File parent, String child) // 根据父级文件 + 子路径 
  - new File(String parent, String pathname) // 根据父级路径 + 子路径
  - createNewFile 创建新文件，new File 只是在内存中创建一个 File 对象，调用该方法才会真正把文件输出到磁盘中。
- 获取文件信息
  - getName
  - getAbsolutePath
  - getParent
  - length：字节
  - exists
  - isFile
  - isDirectory
- 目录操作(在java 中，目录也被当做文件)
  - mkdir：创建一级目录
  - mkdirs：创建多级目录，多级必须用这个
  - delete：
## IO 流原理以及流的分类
- 原理
  1. IO 用于处理数据传输，如读写文件，网络通讯。
  2. Java 程序中，对于数据的输入/输出是以流的方式进行。
  3. Java.io 包下提供各种流类和接口，以获取不同类型的数据，并通过方法输入或输出数据。
- 流的分类
  - 按操作数据单位不同分为：字节流，字符流
  - 按数据流的流向不同分为：输入流，输出流
  - 按流的角色不同分为：节点流，处理流（包装流）
  - 抽象基类
    |  抽象基类   | 字节流  | 字符流  |
    |  ----  | ----  | ----  |
    | 输入流  | InputStream | Reader |
    | 输出流  | OutputStream |Writer |
    1) java 中 IO 流涉及到 40 多个类，都是由这 4 个抽象基类派生
    2) 派生的子类都是以其父类名作为子类后缀。


## 输入流*
### InputStream
- 按字节读取，汉字会出现乱码（如 utf-8 是3个字节确定一个汉字）
- 输入流不使用时，需要关闭。
#### FileInputStream
- read()：从输入流读取一个字节，如果没有收入可用，此方法将被阻止，如果返回 -1，表示文件读取完毕。单个字节读取，效率比较低。
- read(byte[] buf)：读取数组 buf 长度的字节数，返回实际读取大小，返回值如果是 -1，表示读取完毕。


### Reader
- 按字符读取，汉字不会出现乱码。

#### FileReader
- new FileReader(File/String)
- read()：每次读取单个字符，文件末尾返回 -1.
- read(char[])：批量读取多个字符到数组，字符不能再用 type 类型数组，返回读取的字符数，文件结尾返回 -1.


## 输出流*
### OutputStream
#### FileOutputStream
- FileOutputStream(fileName); //会覆盖原来的内容
- FileOutputStream(fileName, true); //会追加在原来的内容后面。
- write(int c)：写入一个字节。
- write(byte[] c)：写入一个字节数组
- write(byte[] c, int off, int len)：写入一个字节数组

> 文件拷贝
> 1. 创建文件输入流，将文件写入到程序
> 2. 创建文件输出流，将读取的文件数据写入到指定的文件。
> ```java
> FileInputStream fis = null;
> FileOutputStream fos = null;
> try {
>   fis = new FileInputStream("原文件目录");
>   fos = new FileOutputStream("目标文件目录");
>   // 定义一个数组，提高读取效果。
>   byte[] buf = new byte[1024]; 
>   int readLen = 0;
>   while (readLen = fis.read(buf) != -1){
>       // 不要用这个，可能最后读取的字节数和数字长度不一致，导致拷贝的数据出现异常。
> //    fos.write(buf); 
>     fos.write(buf, 0, readLen);
>   }
> } catch (IOException){
> } finally {
>   fis.close();
>   fos.close();
> }
> ```

### Writer
#### FileWriter
- new FileWriter(File/String)：覆盖模式创建
- new FileWriter(File/String, true)：追加模式创建
- write(int)：写入单个字符
- write(char[])：写入指定数组
- write(charp[], off, len)：写入指定数组的指定部分
- write(String)：写入整个字符串
- write(String, off, len)：写入字符串指定部分

> 注意：
> FileWriter 使用后，必须要关闭或刷新，否则写入不到指定文件！

## 节点流和处理流
1. 节点流可以从特定的数据源读写数据
2. 处理流是“连接”在已存在的流之上，为程序提供更强大的读写功能。

### 区别
1. 节点流逝底层流，直接跟数据源相连。
2. 处理流包装节点流，可以消除不同节点流的实现差异，提供更方便的方法来完成输入输出。
3. 使用了修饰器设计模式，不会直接与数据源相连。

### 处理流功能主要体现
1. 性能提升：主要以增加缓冲的方式来提高输入输出的效果
2. 操作便捷：提供一系列方法来一次收入输出大批量的数据，使用更加灵活方便。

### BufferedReader 和 BufferedWriter
- BufferedReader 继承 Reader，封装了一个 Reader 类型的成员变量，构建 BufferedReader 时，可以根据需要传任一 Reader，封装了一个 的子类给该对象，底层其实是利用这个对象对流进行操作
- BufferedWriter 继承 Writer，同上。
- 属于字符流，按照字符来读取数据
- 关闭处理流，只需要封闭外层流即可 
- 不要去操作二进制文件(声音、视频、doc...)，可能造成文件损坏。

### BufferedInputStream 和 BufferedOutputStream
- BufferedInputStream -> FilterInputStream -> InputStream,封装了一个 InputStream 类型的成员变量（在 FilterInputStream 定义）。
- BufferedOutputStream -> FilterOutputStream -> OutputStream,封装了一个 OutputStream 类型的成员变量（在 FilterOutputStream 定义）。
- 是字节流，实现缓冲输入输出流，可以将多个字节写入底层输入输出流中，不必对每次字节写入调用底层系统。

### ObjectInputStream 和 ObjectOutputStream
- ObjectInputStream 继承 InputStream，ObjectOutputStream 继承 OutputStream
- 可以保存值和数据类型，就是想基本数据类型或者对象进行序列化和反序列化。
- 序列化：在保存数据时，保存数据的值和数据类型
- 反序列化：在恢复数据时，恢复数据的值和数据类型，读取顺序必须和序列化时的顺序一致。
- 需要对象支持序列化，值必须让其类是可以序列化的，该类必须实现 Serializable 接口或 Externalizable 接口（该接口需要实现方法，所以一般用 Serializable）。

### 细节和注意事项
1. 序列化和反序列化的顺序必须一致
2. 需要实现 Serializable 接口才能进行序列化
3. 建议添加 SerialVersinUID ,为了提交版本的兼容性。
4. 序列化对象时，默认会序列化所有属性，transient 和 static 修饰的属性不会被序列化。
5. 序列化对象时，要求序列化的属性也实现 Serializable 接口。
6. 序列化具有可继承性，父类实现了 Serializable 接口，其子类都可以进行序列化。

## 其他
### 标准输入输出流
#### 标准输入 System.in
- 编译类型 InputStream
- 运行类型 BufferedInputStream
- 键盘

#### 标准输出 System.out
- 编译类型 PrintStream
- 运行类型 PrintStream
- 显示器

### 转换流
- 可以把字节流转换成字符流
- 可以指定编码方式
- FileInputStream -> InputStreamReader -> BufferedReader
- OutputStreamWriters

### 打印流
#### 字节打印流，PrintStream
- 在默认情况下，是标准输出，即显示器。
- System.setOut 可以修改打印流输出位置， System.setOut(new PrintStream("e:\\f1.txt"));System.out.println("hello");
#### 字符打印流，PrintWriter

## Properties 类
- 专门用于读写配置文件的集合类
- 键值对不需要有空格，值不需要用引号，默认类型 String

### 常用方法
- load :加载配置文件的键值对到 Properties 对象
- list :将数据显示到指定设备、流对象
- getProperty : 根据键获取值
- setProperty : 设置键值对到 Properties 对象
- store : 将 Properties 中的键值对存储到配置文件（idea 中保存信息存在中文，会存储为 Unicode 值）。

![IO流](F:/note/file/java/MyJava//img/IO流.png)
