#### 第五章：面向对象编程（基础篇）

##### 栈和堆的区别

栈：储存变量名并且指向实际内容的地址（堆）。

堆：存储实际的数据。

##### static

* static变量在类中只会被实例化一次（实现单例设计模式）。
* static方法无法被非static方法进行调用。
* static变量可以直接使用类进行访问。

##### private

* 构造方法私有化无法被实例化，只能通过私有化对象变量并且通过类来进行访问（一个类只有一个实例对象）。

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20240926131837683.png" alt="image-20240926131837683" style="zoom:50%;" />

* 只要是属性，必须全部进行封装。

##### 代码块分类

* 静态代码块：使用static修饰的代码块；只执行一次。
* 实例代码块：每次创建对象时执行。
* 构造器：构造方法；在实例化时进行执行。
* 方法代码块：在调用方法时进行调用。

执行顺序：静态代码块 > 实例代码块 > 构造器 > 方法代码块。

##### String常见的方法

* charAt()：查询指定位置的字符。
* equals()：判断两个字符是否相等。
* subString()：字符串的截取。
* 字符串的搜索：
  * indexOf和lastIndexOf()：搜索字符串，返回下标。
  * startWith和endsWith()
  * contains()：判断是否包含一个字符串。
* trim()：去掉前后的空字符。
* replace和replaceAll：替换字符。
* split()：字符串的切割。
* join()：将字符串数组组合。
* toUpperCase和toLowerCase：切换大小写。
* toCharArray()：将字符串转换为字符数组。

#### 第六章：面向对象编程（进阶篇）

##### 继承

* 子类实例化时会先实例化父类。
* 子类的构造方法中默认第一行会有父类的无参构造方法super()。

##### 转型

* 向上转型：子类转为父类。
* 向下转型：父类转为子类。
* 想要向下转型，需要先向上转型，再向下转型。

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20240928224030931.png" alt="image-20240928224030931" style="zoom:50%;" />

##### 抽象类和接口

* 抽象类：包含一个抽象方法的类。
* 接口：全局常量、抽象方法（jdk1.8后面新增：普通方法和静态方法）。

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241001162248414.png" alt="image-20241001162248414" style="zoom:40%;" />

* 一个类不要去继承已经实现好的类，要么继承抽象类，要么实现接口（优先）。

##### final

* final修饰的方法无法被子类覆写。
* final修饰的变量无法被修改，变成一个常量。

#### 第七章：异常的捕获和处理

##### 异常的捕获

* 如果有多个异常，不要直接使用Exception捕获全部异常。

* 小的异常必须在打的异常之前被捕获，因此**一般在开发中，直接使用Exception进行处理**。

##### throw和throws的区别

* throw抛出一个异常对象。
* throws要求方法在调用的时候必须在调用处使用try……catch来处理异常。

一般在实际开发中使用类来抛出异常类，主方法中使用try……catch来接受异常。

##### Exception和RuntimeException

* RuntimeException是Exception的子类。

* Exception定义了必须处理的异常，而 RimtiitieExcqrtion定义的异常可以选择性地进行处理。

#### 第八章：包及访问控制权限

##### 访问控制权限

* 如果不同包之间互相访问时，必须使用public来修饰类名。
* 一个包中只能有一个public class。

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241007202255348.png" alt="image-20241007202255348" style="zoom:50%;" />

##### 命名规范

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241007202427003.png" alt="image-20241007202427003" style="zoom:50%;" />

#### 第九章：多线程

##### 多线程的创建

* 继承Thread类：

  * 子类必须覆写run方法。
  * 使用start方法来执行run，实例只能使用一次start方法，否则会报错。

* 实现Runable接口：

  * 覆写run方法。

  * 实例对象之后传入Thread类中，通过start启动多线程。

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241009102154601.png" alt="image-20241009102154601" style="zoom:33%;" />

  * 直接使用Thread的重载方法：Thread(Runable target, String name)

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241015154822146.png" alt="image-20241015154822146" style="zoom:50%;" />

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241015155331199.png" alt="image-20241015155331199" style="zoom:50%;" />

* Thread和Runable（推荐）的区别：
  * Runable可以实现同一资源的共享使用。
  * Thread是Runable接口的子类。

* 如果想要获取返回值使用Callable接口。

##### 同步与死锁

* 多个进程访问同一资源时需要使用同步操作：
  * 同步代码块：synchronized（锁定对象）{代码}
  * 同步方法：public synchronized 返回类型 方法名称() {代码}
* 过多的同步操作会导致死锁问题。

##### wait()和notify()的使用

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241009212720620.png" alt="image-20241009212720620" style="zoom:40%;" />

##### 线程的生命周期

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241009212932581.png" alt="image-20241009212932581" style="zoom:45%;" />

* 上述的suspend，resume和stop不推荐使用。线程的消亡：run方法结束；调用stop方法。
* 实际开发中推荐在继承了Runable的类中设置一个标志位来控制线程的暂停。

#### 第十章：泛型

避免类转换异常。

##### 上限和下限

* 上限（extends）：设置泛型的父类，传进来的是子类。
* 下限（super）：设置泛型的子类，传进来的是父类。

实际开发中可以定义一个接口，使用接口来限制传进来的类（接口的子类）。

#### 第十一章：Java常用类库

##### StringBuffer类

* StringBuffer类可以进行修改。

* String、StringBuffer和StringBuilder的区别：

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241015171403335.png" alt="image-20241015171403335" style="zoom:40%;" />

##### Runtime类

* 可以使用Runtime实例（Runtime.getRuntime()）来进行手动垃圾收集。
* java中JVM的内存划分：
  * Eden（较高频率收集）：存放新产生的对象，**多次垃圾收集之后仍存在将向旧生代转移**。
  * 存活区：Eden和旧生代的中间交换区域。
  * 旧生代（较低频率收集）：存放生命周期较长的对象。

##### 国际化程序

* 通过创建不同的属性文件来实现不同语言的展示。

##### system类

* 获取时间：System.out.println("当前时间的毫秒数: " + currentTimeMillis);
* 获取环境变量：String path = System.getenv("PATH");

##### 日期操作类

* Data类

* Calendar类：结合Calendar实例和StringBuffer实例。

* DataFormat类：将日期转换为指定格式。

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241015201154256.png" alt="image-20241015201154256" style="zoom:40%;" />

* SimpleDateFormat类：将日期转换为指定格式。

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241015201342638.png" alt="image-20241015201342638" style="zoom:40%;" />

  日常开发中常使用parse方法来将字符串转换为Date类型。

##### Math类

* 该类都是直接使用静态方法直接实现功能。

##### Random类

##### NumberFormat类

* 使用静态方法获得实例。

##### Array类

* 排序：Array.sort().

##### Comparable接口

* 如果需要使用Array.sort()对对象数组进行排序，对象必须实现Comparable接口。

##### Comparator类

* 在定义类的时候忘记实现Comparable接口的时候使用，属于一种补救方式。

##### 定时调度

* 需要使用Timer和TimerTask抽象类（实现run方法）来完成。

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241015223520726.png" alt="image-20241015223520726" style="zoom:40%;" />

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241015223544757.png" alt="image-20241015223544757" style="zoom:40%;" />

#### 第十二章：Java IO

重点掌握五个类一个接口：

* 五个类：File、OutputStream、InputStream、Writer和Reader。
* 一个接口：Serializable。

| 分类         |     字节输入流（Byte Input Stream）      |      字节输出流（Byte Output Stream）      | 字符输入流（Character Input Stream） | 字符输出流（Character Output Stream） |
| ------------ | :--------------------------------------: | :----------------------------------------: | :----------------------------------: | :-----------------------------------: |
| **基类**     |              `InputStream`               |               `OutputStream`               |               `Reader`               |               `Writer`                |
| **单位**     |               字节（8位）                |                字节（8位）                 |        字符（16位，Unicode）         |         字符（16位，Unicode）         |
| **常见子类** | `FileInputStream`、`BufferedInputStream` | `FileOutputStream`、`BufferedOutputStream` |    `FileReader`、`BufferedReader`    |    `FileWriter`、`BufferedWriter`     |

##### File类

* 在实际生产中一般使用File.separator常量作为分割符，这样可以兼容Linux和Windows系统。

##### RandomAccessFile类

对文件内容进行操作

* 在写入内容时需要固定字节大小；读取的时候通过跳过字节来读取指定的内容。

##### 重点：字节流与字符流基本操作

基本操作流程：

1. 使用File类打开文件
2. 使用字节流或字符流的子类指定输出位置
3. 进行读/写操作
4. 关闭输入/输出

* 字节流：
  * OutputStream类：使用FileOutputStream(File)进行实例化。
  * InputStream类：使用FileInputStream(File)进行实例化。
* 字符流：
  * Writer类：使用FileWriter(File)进行实例化。
  * Reader类：使用FileReader(File)进行实例化。
* 字节流和字符流的区别：
  * 字节流直接进行输出，不会保存在缓冲区中。（**推荐**）
  * 字符流先保留在缓冲区中，关闭Writer或者.flush()后输出。

##### 内存操作流

一般在生成一些临时信息时才会使用。一切操作都在内存中进行，不会产生文件。

##### 管道流

将管道输出流接在管道输入流上：在操作时只需要使用PipedOutputStream类中提供的connection 方法。

##### 打印流

* 传入一个OutputStream对象
  * print()：直接将指定内容打印在文件中。
  * printf()：使用格式化将内容打印在文件中。

##### System对IO的支持：三个常量

* System.out实际上是OutputStream的对象。
* System.err不希望被用户看见报错信息。
* System.in实际上是InputStream的对象从键盘上获得内容。
* 输入/输出重定向：
  * 将默认的输入输出位置进行改变。

##### BufferdReader类

从缓冲区中读取内容。

* BufferedReader的构造方法中只能接受字符输入流的实例，所以必须使用字符输入流-字节输入流。

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241016201425751.png" alt="image-20241016201425751" style="zoom:40%;" />

* 可以使用BufferedReader类来对用户的输入进行接受(.readLine())。

##### 数据操作流

格式化输出内容。

* DataOutputStream类：使用.writeXxx()方法进行输出。
* DataInputStream类：使用.readXxx()来读取内容。

##### 合并流

从两个InputStream读取内容合并到一起。

##### 压缩流

* ZipOutputStream类（压缩）：接受一个OutputStream类，先.putNextEntry，再.write()。
* ZipFile类：用于解压缩zip文件。

##### 回退流

PushbackInputStream类是InputStream的子类，使用.unread()方法来避免内容。

##### 字符编码

在实际开发中推荐使用UTF-8编码。

##### 对象序列化

序列化：指将对象以字节流的形式保存下来，只将对象的数据进行序列化。

* ObjectOutputStream类：构造方法new FileOutputStream(File f)。使用.writeObject方法来序列化对象。
* ObjectInputStream类：构造方法new FileInputStream(File f)。使用.readObject方法来反序列化对象。

#### 第十三章：Java类集框架

##### Collection接口

* List接口：允许出现相同元素
* Set接口：不允许出现相同元素
* SortedSet接口：
* Map接口

##### List接口

* ArrayList类：可以动态变换长度的数组。
* Vector类：旧版本的ArrayList。
* LinkedList类：链表

##### Set接口

* HashSet类：内部元素无序
* TreeSet类：对内部元素进行排序，如果传入对象类的话需要提前对类继承Compareable<T>实现compareTo方法。
  * 在进行是否有重复对象时，必须对对象类中重写hashCode()、equals()方法。因此一个完整的类最好覆写hashCode()、equals()和toString()方法。

##### SortedSet

排序好的Set类。

##### 集合的输出

* Iterator类：可以直接使用Collection接口中的.iterator()方法进行实例化。

* ListIterator类：可以实现双向输出。只能使用List实例的.listIterator()方法。如果想要实现从后往前输出内容，必须先进行由前往后的输出。
* Enumeration类：使用Vertor.elements()进行实例化。

##### Map接口

* HashMap类：元素无序。

* TreeMap类：元素有序。

* Map的输出：

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241019154804155.png" alt="image-20241019154804155" style="zoom:40%;" />

##### Collections类

提供工具来对Collection子类进行操作（如一次性添加多个元素）。

#### 枚举

* **枚举常量都是枚举类型的实例,里面的常量都属于该类，因此可以用来限制取值**

* 一个枚举类可以实现一个接口或者直接定义一个抽象方法，但是枚举中每个元素都必须分别实现全部抽象方法。

#### Java反射机制

反射机制：通过包的路径来实例化一个对象，并且获得该类的构造方法、属性和方法等信息。

##### Class类

* 所有类的对象实际上都是Class类的实例。
* 可以使用Class.forName来指定对象类，使用newInstance来实例化对象。

##### Constructor类

* 可以使用getConstructors方法来得到构造方法数组，通过newInstance方法来实例化对象。

##### 工厂模式进阶版

* 使用Class.forName来创建实例，这样就可以不在工厂类中反复地添加文件。
* 使用配置文件完成类名和路径的对应关系，这样就不用记住每一个类的路径。

#### Annotation与Lamda

##### Annotation（内置注解）

* @Override
* @Deprecated
* @SuppressWarnings

##### 自定义Annotation

使用@interface来修饰相当于是继承了Annotation类。

* @Retention：定义注解的保留策略，即注解在哪个阶段被保留，决定哪些注解可以被使用
  * **RetentionPolicy.SOURCE**: 只在源码中保留，编译后丢弃（编译器处理）。
  * **RetentionPolicy.CLASS**: 编译时保留在类文件中，但运行时不可见（默认行为）。
  * **RetentionPolicy.RUNTIME**: 在运行时也可通过反射获取。
* @Target：指定注解可以应用的目标（如类、方法、字段等）。
* @Inherited：指示某个注解可以被子类继承。如果父类使用了`@Inherited`修饰的注解，子类自动继承此注解。
* @Documented：指示该注解是否包含在Javadoc文档中。

##### Lambda表达式

相当于构造了一个匿名内部类，重写了里面的抽象方法。

##### 方法引用

将一个类的方法引用到另外一个类的方法上。

##### 内建函数式接口

可以结合Lambda表达式进行使用。

#### 网络编程

##### URL

* 可以使用URL来定位网上的资源，使用IO流进行读取。

##### TCP

* 每一个Socket都表示一个连接的客户端。
* 可以使用多线程来连接多个客户端。

#### 设计模式

