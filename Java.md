## 记录java相关！
- [public、privite、protected修饰符](#public-privite-protected修饰符)
- [final修饰符](#final修饰符)
- [static修饰符](#static修饰符)
- [封装、继承、多态](#封装继承多态)
- [内部类](#内部类)
- [泛型](#泛型)
- [枚举](#枚举)
- [多线程](#多线程)
- [类加载过程](#类加载过程)
- [多线程](#多线程)
- [String、StringBuffer和StringBuilder的区别](#String-StringBuffer-StringBuilder的区别)
- [ArrayList、LinkedList以及Vector的区别](#ArrayList-LinkedList以及Vector的区别)
- [java中try-catch-finally的执行顺序](#java中try-catch-finally的执行顺序)
- [java中八大基本类型](#java中八大基本类型)
- [反射](#反射)
- [关于线程](#关于线程)
- [Volatile](#Volatile)
- [Synchronized](#Synchronized)
- [原子操作](#原子操作)
- [写一个死锁](#写一个死锁)
- [synchronized关键字实现两个线程交替打印奇偶数](#synchronized关键字实现两个线程交替打印奇偶数)
- [lock三个线程依次交替打印1到100](#lock三个线程依次交替打印1到100)
- [java异常处理](#java异常处理)
- [java是值传递还是引用传递](#java是值传递还是引用传递)

### public-privite-protected修饰符
在java中，public、privite、protected三种修饰符用来修饰类以及类的成员变量和方法。</br>
修饰类的成员变量以及方法时：privite，变量和方法只能在类中可见，对子类以及其他包中的类是不可见的，方法不可见，那么子类也无法对父类中privite修饰的方法进行重写。public，对子类以及其他包里的类都可见。protected，子类以及同一个包中的类均可访问(当一个类被public修饰，其方法A被protected修饰，那么该方法只允许同一包中的类或者其他包的子类访问)。可以这么理解，只要方法和变量不是private修饰，那么就对其子类可见。</br>
修饰类时：privite修饰类，那么类中的所有数据(变量，方法)都将被隐藏，一般没有太大意义，因为我们定义一个类，就是用来创建对象并调用的，弄成隐藏，还有什么意义（除了内部类，一般的类也不允许这么修饰）。public修饰类，该类对所有类可见。protected修饰类，该类只能对同包中的类可见。

* 默认情况下，作用域相当于protected；
* 类的权限设定会约束成员的权限，这应该很好理解。

### final修饰符

final字段可修饰类、方法以及变量。

* 对于一个final变量，如果是基本数据类型的变量，那么其值在初始化之后是不可以改变的。一个既是static又是final修饰的变量只占据一段不能改变的内存；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。
* 当用final修饰一个类时，表明这个类不能被继承。final类中的所有成员方法都会被隐式地指定为final方法,不能被子类重写。<br/>
* 使用final方法的原因有两个。第一个原因是把方法锁定，以防任何继承类修改它的含义；第二个原因是效率。在早期的Java实现版本中，会将final方法转为内嵌调用。但是如果方法过于庞大，可能看不到内嵌调用带来的任何性能提升（现在的Java版本已经不需要使用final方法进行这些优化了）。类中所有的private方法都隐式地指定为final。

### static修饰符

通常情况下，会出现多个类在内存区域共享一个数据，这是我们就通过static来修饰这个数据。static可以修饰类，方法、变量以及代码块，java虚拟机会为static修饰的变量等分配唯一的内存区域，且周期跟随进程。使用static需要注意以下几点：

* 静态成员同样遵循public、private、protected修饰符的约束；
* 静态方法中不可以使用this字段；
* 静态方法不可以直接调用非静态方法；
* 方法中的局部变量不能声明为static，即使是静态方法也不行。

### 封装继承多态
java是一种面向对象的语言，在它的眼里，万物皆对象。而面向对象的三大特点，就是封装、继承、多态。<br/>
封装：面向对象编程的核心思想，将对象的属性和行为封装起来，其载体为类，只为外部提供相关调用，而隐藏具体的实现细节。<br/>

继承：类与类之间存在着某种关系，继承就是特殊的一种类与类之间关系。<br/>
* 子类可以继承父类的方法和属性，也可以增加父类不具备的属性和方法，或者直接重写父类中的某些方法。
* 方法重写(覆盖)：子类中将父类的成员方法的名称和参数保留，重写实现内容，更改修饰权限，或返回值。如果只重写实现内容，那么就叫重构。值得注意的是，更改修饰权限只能从小的范围到大的范围，父类中为protected，那么子类只能修改为public；修改返回类型时，子类的返回类型必须是父类的返回类型的子类，而不能随便类似的将返回类型int更改为string。
* 在实例化子类时，会自动调用父类的无参构造函数，先实例化父类，再实例化子类；
* Object类是所有类的父类；
* 向上转型：Animal animal = new Dog();向下转型：Dog dog = (Dog) animal;
* 使用instanceof判断对象类型；
* 方法重载：同一个类中允许存在一个以上的同名方法，只要这些方法的参数个数、类型、顺序不同即可。可以知道，编译器是利用方法名，方法参数的类型、个数以及顺序来确定类中的方法是否唯一的。

多态：将父类对象，应用于之类的特征，就是多态。多态的实现依赖于抽象类和接口。<br/>
* 非抽象类中不可以有抽象方法，抽象类中可以有非抽象方法；
* 接口是存粹的抽象类，所有的方法都没有实体；
* 接口中定义的方法必须是public或abstract；
* 在接口中定义的任何字段都自动是static和final的。

### 内部类
如果在一个类中在定义一个类，则将在该类中再定义的那个类称为内部类。内部类包括成员内部类、局部内部类、匿名内部类。<br/>
成员内部类：<br/>
* 内部类中可以随意使用外部类的成员变量和成员方法，尽管这些类成员被修饰为private，但外部类则不能直接访问内部类成员。
* 在实例化内部类对象时，不能在new操作符之前使用外部类名称实例化内部类，而是应该使用外部类的对象来创建其内部类对象。
   ```java
   JavaTest out = new JavaTest();
   JavaTest.inClass inClass = out.new inClass();
   ```
 * 内部类对象依赖于外部类对象，除非已经存在一个外部类对象，否则类中不会出现内部类对象。
 
局部内部类：在类的方法中或任意作用域中定义的类，就叫局部内部类。值得注意的是，在方法中定义的局部内部类只能访问方法中的final对象，这是因为在方法中定义的局部变量相当于一个常量，它的生命周期超过了方法运行的周期，所以不能在局部类中改变局部变量的值。<br/>
匿名内部类：没有名称的内部类，所以叫匿名内部类。匿名内部类编译后，会产生以“外部类名$序号”为名称的.class文件，序号以1到n排列，分别表示1到n个匿名内部类。<br/>
```java
return new A(){

};
```
静态内部类：给内部类添加static修饰符，就变成了静态内部类。<br/>
* 如果创建静态内部类对象，不需要依赖其外部类；
* 不能从静态内部类的对象中访问非静态外部类的对象。
* 
### 泛型
泛型是Java SE1.5的新特性，泛型的本质是参数化类型，也就是说所操的数据类型被指定为一个参数。这种参数类型可以用在类、接口和方法的创建中，分别称为泛型类、泛型接口、泛型方法。 Java语言引入泛型的好处是安全简单。<br/>
在Java SE 1.5之前，没有泛型的情况的下，通过对类型Object的引用来实现参数的“任意化”，“任意化”带来的缺点是要做显式的强制类型转换，而这种转换是要求开发者实际参数类型可以预知的情况下进行的。对于强制类型换错误的情况，编译器可能不提示错误，在运行的时候出现异常，这是一个安全隐患。<br/>
泛型的好处是在编译的时候检查类型安全，并且所有的转换都是自动和隐式的，提高代码的重用率。<br/>

* 泛型的类型参数只能是类类型（包括自定义类），不是简单类型。

* 同一种泛型可以对应多个版本（因为参数类型是不确的），不同版本的泛型类实例是不兼容的。

* 泛型的类型参数可以有多个。

* 泛型的参数类型可以使用extends语句，例如。习惯上称为“有界类型”。

* 泛型的参数类型还可以是通配符类型。例如Class<?> classType = Class.forName("java.lang.String");

泛型擦除以及相关的概念：<br/>
泛型信息只存在代码编译阶段，在进入JVM之前，与泛型关的信息都会被擦除掉。<br/>

在类型擦除的时候，如果泛型类里的类型参数没有指定上限，则会被转成Object类型，如果指定了上限，则会被传转换成对应的类型上限。<br/>

Java中的泛型基本上都是在编译器这个层次来实现的。生成的Java字节码中是不包含泛型中的类型信息的。使用泛型的时候加上的类型参数，会在编译器在编译的时候擦除掉。这个过程就称为类型擦除。<br/>

类型擦除引起的问题及解决方法：<br/>

* 先检查，在编译，以及检查编译的对象和引用传递的题

* 自动类型转换

* 类型擦除与多态的冲突和解决方法

* 泛型类型变量不能是基本数据类型

* 运行时类型查询

* 异常中使用泛型的问题

* 数组（这个不属于类型擦除引起的问题）

* 类型擦除后的冲突

* 泛型在静态方法和静态类中的问题

值得注意的两个关键字：<br/>
* extends ：<? extends T> 上边界，必须是T的子类；
* super ：<? super T> 下面界，必须是T的超类；

### 枚举
通常情况下，我们将常量放置在接口中，从JDK1.5开始，引入了枚举的概念。
```java
enum  EnumTest{
        A("s1"),
        B("s2"),
        C("s3");

        private String name;
        //构造方法的修饰符必须为private
        private EnumTest(String name) {
            this.name = name;
        }
    }
```
一个枚举类成员都可以看作是枚举类型的实例，默认被final、public、static修饰。<br/>
常用方法：
* values()：将枚举类型成员以数组的形式返回；
* valusOf():将普通字符串转换成枚举实例；
* compareTo()：比较两个枚举对象在定义时的顺序；
* ordinal():返回枚举成员的位置索引。

枚举的优点：
* 类型安全；
* 紧凑有效的数据定义；
* 可以和程序其他部分完美的交互；
* 运行效率高。

### 多线程
java 内存模型：<br/>

![avatar](/image/thread_1.png)

一个进程有着系统为其分配的内存空间，进程中又可以开辟着多个线程同步的处理着工作。所有的共享变量存储在主内存里，每个线程有着自己的工作内存，工作内存中保存了被该线程使用到的变量的主内存的拷贝，线程多变量的所有操作必须在工作内存中完成，而不能直接读写主内存中的变量。当然，线程间直接是不能访问彼此的工作内存的，需要通过主内存来进行传递。主内存与工作内存之间的操作：<br/>
* Lock(锁定)：作用于主内存的变量，表示一个变量被一个线程独占。
* unLock(解锁)：作用于主存的变量，表示把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程所使用。
* read(读取)：作用于主存变量，把一个变量从主存传输到线程的工作内存；
* load(载入):作用于工作内存的变量，把read操作的变量从工作内存放入工作内存的变量副本中；
* use(使用)：作用于工作内存中的变量，每当虚拟机遇到一个给变量赋值的字节码的指令时，就需要将工作内存中的变量传递给执行引擎；
* assign(赋值)：作用于工作内存的变量，把执行引擎收到的变量的值传递给工作内存中的变量；
* store(存储)：作用于工作内存的变量，将工作内存中的变量传递到主内存中；
* write(写入)：把store操作的变量写入到主内存中。

### 类加载过程

类的生命周期：<br/>

![avatar](/image/class_1.png)

类的生命周期包括上面7个状态，其中验证、准备、解析三个阶段统称为连接。前五个阶段为类加载，下面分别介绍：<br/>
#### 加载：
* 通过一个类的全限定名来获取此类的二进制字节流；
* 将这个二进制流所代表的静态存储结构转化为方法区的运行时数据结构；
* 在java 堆中生成一个代表该类的java.lang.class对象，作为方法区这些数据的访问入口；

#### 验证：
验证是确保二进制流文件中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机的安全。java语言相对于c、c++来说是相对安全的，使用纯粹的java语言是无法做到访问数组越界、对象转型不成功等情况，这些会在.java文件被编译成.class文件时就报异常。但是针对java虚拟机来说，它不会在乎二进制流文件是如何产生的，可以通过任何途径生成class的文件，因此验证就显得很必要。
* 文件格式验证：验证字节流是否符合class文件的规范，并且能被当前版本的的虚拟机处理。保证输入流能被正确的解析并存储于方法区中，格式上符合描述一个java类型信息的要求。进过该验证阶段后，字节流才会进入内存的方法区中进行存储。
* 元数据验证：对字节码描述的信息进行语法验证，以符合java语法规范。比如这个类是否有父类(除了Object类外，所有的类都应当有父类)、或者这个类是否继承了不能被继承的父类等。
* 字节码验证：在前面对元数据的语法校验后，本阶段对方法体语法进行校验，以保证类的方法体在运行时不破坏虚拟机。例如保证任意时刻操作数栈的数据类型与指令代码能配合工作，不会出现在操作栈中放了一个int类型的数据，使用时却按long类型加入到本地变量表中等。
* 符号引用验证：该验证发生在将符号引用转换为直接引用的过程中，也就是将在解析阶段的时候。符号引用验证可以看作是对类自身以外(常量池中的各种符号应用)的信息进行匹配性的检测，以确保解析功能的正常运行。
值得一提的是，验证阶段对类加载很重要，但不是必须的。对于被多次重复验证和使用后的代码，可以通过-Xverify:none来跳过验证，以缩短虚拟机类加载机制的时间。

#### 准备：
准备阶段是正式为类变量分配内存并设置初始值的阶段，这些内存将在方法去得到分配。需要注意的是，这时候进行内存分配的只包括类变量(被static 修饰)，而不包括实例变量，实例变量将会在对象实例化时随着对象一起分配到java堆中。其次这里说的初始值，通常情况下是数据类型的零值，假设一个数据类型被这样定义：privite static int number = 123,那么number在分配内存时初始值为0而不是123，因为这时候并没有执行java方法，将number赋值是在程序被编译后。

#### 解析：
解析阶段是将常量池中的符号引用转换为直接引用的过程。在Class文件中，符号引用是以CONSTANT_Class_info等类型的常量出现。关于二者：
1、符号引用：以一组符号来描述对应的目标，符号可以是任何形式的字面量，只要使用时能无歧义的定位到目标即可。符号引用于虚拟机的内存布局无关，应用的目标不一定已经加载到内存中。<br/>
2、直接引用可以是直接指向目标的指针、相对偏移量或一个能直接定位到目标的句柄。直接引用是于虚拟机的内存布局相关的，同一个符号应用在不同的虚拟机上翻译出来的直接引用一般不同，如果有了直接应用，那么被引用的对象一定存在于内存中。<br/>
解析动作主要针对类或接口、字段、类方法以及接口方法四类符号应用进行。

* 类或者接口的解析：<br/>
假设当前所处的类D，如果要把一个从未解析过的符号引用N解析为一个类或者接口C的直接引用，那么需要经过下面几步骤：<br/>
1、如果C不是一个数组类型，那么虚拟机将会把代表N的全限定名传给D的类加载去加载C。在这个过程中，可能会触发其他事件，比如加载该类的父类或实现的接口，一旦该过程出现异常，则宣告解析过程失败。<br/>
2、如果C市一个数组，且数组元素为对象，那么将会按照上面的方法加载数组元素类型。<br/>
3、如果上面的过程中没有出现异常，那么C在虚拟机中已经存在有效的类或者接口了。但在解析完之前还要进行符号引用验证，确认C是否具有对D的访问权限。<br/>

* 字段解析<br/>
解析一个从未被解析过的字段，需要先解析字段所属的类或者接口的符号引用。如果解析失败，则抛出异常；如果成功，将该字段所属的类或者接口用C表示，虚拟机规范需要对C进行后续字段的搜索：<br/>
1、如果字段本身都包含了简单名称和字段描述符都与目标相匹配的字段，则返回这个字段的直接引用，结束查询；<br/>
2、否则，如果C中实现了接口，将会按照继承关系从上往下递归搜索其父接口，如果在父接口中包含了简单名称或描述符都与目标相匹配的字段，则返回这个字段的直接引用，结束查询；<br/>
3、否则，如果C中实现了父类，将会按照继承关系从上往下搜索其父类，如果在父类中包含了简单名称或描述符都与目标相匹配的字段，则返回这个字段的直接引用，结束查询；<br/>
4、否则，查询失败，抛出异常。<br/>
在实际情况中，虚拟机可能更加严格，如果同一字段出现在C类中或者其父类、接口中，那么就会报编译异常；

* 类方法解析<br/>
类方法的解析和字段解析一样，首先需要解析所属的类或者接口的符号引用，<br/>
1、类方法和接口方法符号引用的常量是分开的，如果在类方法表中发现索引的C是接口，则抛出异常。<br/>
2、在C中查找是否有简单名称和描述符都与目标相匹配的方法，如果有则返回该方法的直接引用，查找结束；<br/>
3、否则，在类C的父类中递归查找是否有简单名称和描述符都与目标相匹配的方法，如果有则返回该方法的直接引用，查找结束；<br/>
4、否则，在类C实现的接口中递归查找是否有简单名称和描述符都与目标相匹配的方法，如果有则说明C是一个抽象类，这时候查找结束，抛出AbstractMethodError异常；<br/>
5、否则，宣告查询失败。<br/>
最后，如果找到了方法的直接引用，将会对方法进行权限验证，如果发现对这个方法不具备访问权限，抛出异常。<br/>

* 接口方法解析<br/>
同样的，接口方法的解析首先需要解析出接口方法所属类或者接口的符号引用，<br/>
1、如果在接口方法中发现了C是个类而不是接口，直接抛出异常；<br/>
2、否则，在接口C中查找是否有简单名称和描述符都与目标相匹配的方法，如果有则返回该方法的直接引用，查找结束；<br/>
3、否则，在接口C的父接口中递归查找，是否有简单名称和描述符都与目标相匹配的方法，如果有则返回该方法的直接引用，查找结束；<br/>
4、否则，查找失败；<br/>
由于接口方法都是public的，所以不存在权限验证的问题。

#### 初始化：
类的初始化时类加载的最后一步，是真正开始执行类中的java代码。在准备阶段，变量被赋予了初始值，而在初始化阶段，则是根据程序员通过程序制定的计划去赋值初始值和其他资源。或者从另一个方向来说，初始化阶段是执行类构造器clinit()的过程：<br/>
1、<clinit>()方法是由编译器自动收集类中的所有类变量的赋值动作和静态代码块中的语句合并产生的，编译器收集的顺序是由语句在源文件中出现的顺序决定的。<br/>
2、<clinit>()方法与类的构造函数不同，它不需要显示的调用父类的构造器，虚拟机会保证在子类的clinit()方法执行前，父类的clinit()方法已经执行完毕。因此在虚拟机中第一个被执行的clinit()方法的类为Object类。<br/>
3、由于父类的clinit()方法先执行，那么意味着父类中定义的静态代码块要优先于子类的静态代码块先执行。<br/>
4、clinit()方法对于类或者接口来说不是必须的。如果类中没有静态代码块，也没有对变量的赋值操作，那么编译器就不会对类生成clinit()方法。<br/>
5、接口中不能有静态代码块，但可以有对变量的赋值操作，因此接口与类一样，会生成clinit()方法；<br/>
6、虚拟机保证类的clinit()方法在多线程环境中被正确的枷锁和同步。<br/>
   
#### 类加载器：
“通过一个类的全限定名来获取这个类的二进制流”，这个动作在java虚拟机中是有"类加载器"完成的。对于任何一个类，都需要它的类加载器以及其本身来保证它在java虚拟机中的唯一性。换句话说，比较两个类是否相等，需要在这两个类是由同一个类加载器加载的前提下才有意义，否则，即使这两个类来至同一个class文件，只要加载他们的类加载器不一样，则这两个类必然不相等。<br/>
站在java虚拟机的角度，存在两种类加载器。一种是启动类加载器，这个类加载器采用C++语言实现，是虚拟机自身的一部分；另一种就是其他类加载器，由java语言实现，独立于虚拟机外部，并且继承抽象类ClassLoader。<br/>
从java开发的角度，可以划分为下面三种类加载器：<br/>
1、启动类加载器：为虚拟机的自身部分，由C++语言实现，存放在JAVA_HOME/lib路径下。<br/>
2、扩展类加载器：由ExtClassLoader类实现，负责加载JAVA_HOME/lib/ext目录下的，或者被java.ext.dirs系统变量所指定的路径中的类，开发者可以直接使用扩展类加载器；<br/>
3、应用程序类加载器：由AppClassLoader类实现，是ClassLoader类中getSystemClassLoader()方法的返回值。负责加载用户类路径上所有的类库，开发着可以直接使用该类加载器。<br/>

![avatar](/image/class_2.png)

双亲委派模型：如果一个类加载器收到了类的加载请求，它不会马上去尝试加载这个类，而是委派给它的父类加载器去完成，每一层的类加载器都是如此，因此所有的类加载请求最终都会委派到启动类加载器。只有当父类加载器表示无法加载该类时，子类加载器才会自己去加载。<br/>
使用双亲委派模型来组织类加载器之间的关系，保证了java类随着类加载器的层级关系，也具有了优先级的层级关系。比如我们的Object类，无论哪一个类加载器要加载该类，最终都会委派到启动类加载器，最终保证Object类在所有类加载环境中的唯一性。

### String-StringBuffer-StringBuilder的区别

三者都是关于字符串的操作类，先看其结构：
```java
public final class String implements Serializable, Comparable<String>, CharSequence {
    private final char[] value;
    private int hash;
    .....
}
```

```java
public final class StringBuffer extends AbstractStringBuilder implements Serializable, CharSequence {
    private transient char[] toStringCache;
    ....
    public synchronized StringBuffer append(String var1) {
        this.toStringCache = null;
        super.append(var1);
        return this;
    }
}
```

```java
public final class StringBuilder extends AbstractStringBuilder implements Serializable, CharSequence {
    ...
    public StringBuilder append(StringBuffer var1) {
        super.append(var1);
        return this;
    }
}
```
可以看到三者都实现Serializable、CharSequence接口，StringBuffer和StringBuilder都继承AbstractStringBuilder类。三者的存储和操作最终底层都是char数组.但是String里面的char数组是final的,而StringBuffer,StringBuilder不是,也就是说,String是不可变的,想要新的字符串只能重新生成String.而StringBuffer和StringBuilder只需要修改底层的char数组就行.相对来说,开销要小很多.而String的大多数方法都是重新new一个新String对象返回,频繁重新生成容易生成很多垃圾.因此，当涉及到字符串修改比较多的情况下尽量用StringBuffer或者StringBuilder。另外值得注意的是，StringBuffer里所有的方法都被synchronized修饰，是线程安全的，而StringBuilder没有，线程不安全。

### ArrayList-LinkedList以及Vector的区别
ArrayList：
```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
     transient Object[] elementData;
     ......
}
```
ArrayList底层是Object数组存储数据。扩容机制:默认大小是10,扩容是扩容到之前的1.5倍的大小,每次扩容都是将原数组的数据复制进新数组中. 一般在知道了需要创建多少个元素,那么尽量用new ArrayList<>(15)这种明确容量的方式创建ArrayList.避免不必要的浪费.添加:如果是添加到数组的指定位置,那么可能会挪动大量的数组元素,并且可能会触发扩容机制;如果是添加到末尾的话,那么只可能触发扩容机制.删除:如果是删除数组指定位置的元素,那么可能会挪动大量的数组元素;如果是删除末尾元素的话,那么代价是最小的. ArrayList里面的删除元素,其实是将该元素置为null.查询和改某个位置的元素是非常快的( O(1) ).<br/><br/>
Vector:
```java
public class Vector<E>
    extends AbstractList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
     protected Object[] elementData;
}
```
Vector和ArrayList一样，底层都是Object数组存储数据。功能上基本是一样的，但是Vector得所有方法都被synchronized，是线程安全的，而ArrayList是线程不安全的。<br/><br/>
LinkedList:
```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{
    transient int size = 0;
    
    transient Node<E> first;
    
    transient Node<E> last;
 }
```
底层是双向链表存储数据,并且记录了头节点和尾节点。
添加元素非常快,如果是添加到头部和尾部的话更快,因为已经记录了头节点和尾节点,只需要链接一下就行了. 如果是添加到链表的中间部分的话,那么多一步操作,需要先找到添加索引处的元素(因为需要链接到这里),才能进行添加.
遍历的时候,建议采用forEach()进行遍历,这样可以在每次获取下一个元素时都非常轻松(next = next.next;). 然后如果是通过fori和get(i)的方式进行遍历的话,效率是极低的,每次get(i)都需要从最前面(或者最后面)开始往后查找i索引处的元素,效率很低.
删除也是非常快,只需要改动一下指针就行了,代价很小.

### java中try-catch-finally的执行顺序
* try/catch互斥，当try里抛出异常时进入catch代码块，不管try/catch执行如何，finally代码块都会执行；
* 在try/catch中return，在finally执行前会把结果保存起来，将finally里代码执行完后再return，即使在finally中有修改也以try/catch中保存的值为准，但如果是引用类型，修改的属性会以finally修改后的为准；<br/>
eg：
```java
public static int demo()
    {
         int i = 0;
            try {
                i = 2;
                return i;
            } finally {
                i = 12;
                System.out.println("finally trumps return.");
            }       
    }
    
    
//输出结果
------------------
    finally trumps return.
    返回：2
------------------
```
如果是引用类型：
```java
public static User test01() {
        User user = new User("tony");
        try{
            System.out.println("try block: " + user.toString());
            return user;
        } finally {
            System.out.println("finally block: " + user.toString());
            user.name = "jelly";
        }
    }

    static class User {
        String name;
        User(String name) {
            this.name = name;
        }

        @Override
        public String toString() {
            return "name:"+ name;
        }
    }
    
输出：
--------------------- 
try block: name:tony 
finally block: name:tony 
返回：name:jelly
---------------------
```
* 如果try/catch以及finally都有return，直接返回finally中的return。
* 编译器把finally中的return实现为一个warning，所以不建议这么做。

### java中八大基本类型
![avatar](/image/data_title.png) <br/>
我们知道，计算机只能识别二进制格式的数据，不管我们采用什么语言，最终都将转换成类似010101的二进制格式被机器读取。位(bit)是计算机中数据的最小单位, 即二进制位,其值为0和1.一个字节有八个二进制位；<br/>
1byte = 8bit;<br/>
1KB=1024B；<br/>
1MB=1024KB=1024×1024B<br/><br/>
整数：<br/>
![avatar](/image/data_1.png) <br/>
char：字符型，用于存储单个字符，内存中占2个字节，取值范围0~65535，例如，65代表‘A’，97代表‘a’，默认值为空<br/><br/>

boolean：布尔类型，占1个字节，用于判断真或假（仅有两个值，即true、false），默认值false<br/><br/>
浮点数：<br/>
java中浮点数有float、double两种类型<br/>
![avatar](/image/data_2.png)<br/><br/>
十进制整数转化为二进制数<br/>
eg：11表示成二进制数：<br/> 
    11/2=5   余   1<br/>
    5/2=2   余   1<br/>
    2/2=1   余   0<br/>
    1/2=0   余   1<br/>
    0结束         11二进制表示为(从下往上):1011<br/>
    
十进制小数转化为二进制数<br/>
eg:0.9表示成二进制数<br/>
    0.9*2=1.8   取整数部分  1<br/>
    0.8(1.8的小数部分)*2=1.6    取整数部分  1<br/>
    0.6*2=1.2   取整数部分  1<br/>
    0.2*2=0.4   取整数部分  0<br/>
    0.4*2=0.8   取整数部分  0<br/>
    0.8*2=1.6   取整数部分  1<br/>
    0.6*2=1.2   取整数部分  0<br/>
    .........   0.9二进制表示为(从上往下): 1100100100100......<br/>
当对十进制小数进行二进制转换时，多数都会出现死循环的情况，然而浮点数的有效位数和取值范围都是有限制的，因此就出现了浮点数精度的问题。<br/>
浮点数的二进制存储格式为：<br/>
单精度：<br/>
![avatar](/image/data_3.png) <br/>
双精度：<br/>
![avatar](/image/data_4.png) <br/>

符号位：代表浮点数的正负，0正 1负<br/>
指数位：存储科学计数法中指数数据，采用移位存储<br/>
尾数位：尾数数据<br/>
指数位决定范围，尾数位决定精度。<br/><br/>
float:指数8位，指数范围为-2^8-2^8,所以float的取值范围为-2^128-2^128;尾数位为23位，2^23 = 8388608,即float的有效位数为8位，不同的机器精度不一样，但不会超过8位<br/>
double:指数11位，指数范围为-2^11-2^11,所以float的取值范围为-2^2048-2^2048;尾数位为52位。2^52 = 4503599627370496,即float的有效位数为16位，精度16位；<br/>
浮点数内部的存储参考：https://wenku.baidu.com/view/90fad770376baf1ffc4fad7b.html

### 反射
[java反射详解](https://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650242963&idx=1&sn=708e4fa01823b7844494ecb276a3733f&chksm=88638efcbf1407eaf0b5438f824a42a1890ab6a623108990cbbc1d424594ee08e9ff973ff70f&scene=38#wechat_redirect)

### 关于线程
线程是操作系统调度的最小单位，当启动一个java程序时，操作系统会创建一个java进程，并调用main()方法开启main线程。<br/>
#### 线程的状态：
* new,初始状态：线程被创建，但是还没有调用start()方法；
* Runnable，java线程中将操作系统的就绪和运行状态统称为“运行中”
* Blocked，阻塞状态，表示线程阻塞于锁；
* Waiting，等待状态，比如调用了wait()方法，该状态表示当前线程需要等待其他线程做出一些特定的动作(通知或中断)；
* Time_Waiting，等待超时状态，该状态不同于Waiting，它是可以在指定的时间自行返回的；
* Terminated，终止状态，表示当前线程已经执行完毕；<br/>

状态间的切换：<br/>
![avatar](/image/thread_state.png)
### 16、线程安全需要保证的几个基本特性
* 原子性，相关的操作不被其他线程干扰，一般通过同步机制实现，比如关键字(synchronized)；
* 可见性，是一个线程修改了某个共享变量，其状态能够立即被其他线程所知晓，通常被理解为线程本地状态放到主存上，Volatile关键字负责保证可见性。
* 有序性，保证线程内串行语句，避免指令重排等,vilatile关键字可以避免指令重排。

### Volatile
Volatile是轻量级的Synchronized，主要保证两个特性：<br/>
* 可见性：在多处理器开发中保证共享变量的“可见性”。由于Volatile不会引起线程上下文的切换和调度，所以比Synchronized的执行成本更低。java语言允许线程访问共享变量，为了能确保共享变量被准确和一致的更新，线程应该确保通过排他锁单独获得这个变量。通过Volatile关键字修饰的变量，其值被更新后，会通知所有的线程，但这并不表示volatile具有原子性。被volatile修饰的变量转换成汇编代码会多一个Lock前缀的指令，该指令会保证两个原则：(1)、将当前处理器缓存行的数据写回到系统内存；(2)、这个写回内存的操作会使在其他CPU里缓存了该内存地址的数据无效。
* 禁止指令重排，所谓的指令重排，是指普通的变量仅仅保证在该法方法中执行的过程中所有依赖结果的地方获取到正确的结果，但不能保证变量赋值的操作的顺序和代码的顺序是一致的，这就是“线程内表现为串行，从其他线程来看不一定串行”。Volatile通过CPU的内存屏障来实现禁止指令重排。<br/>

Volatile不能保证原子性，那么什么情况下会采用呢：<br/>
* 运行结果并不依赖变量的当前值，或者能够确保只有单一的线程修改变量的值；
* 变量不需要与其他的状态变量共同参与不变约束。<br/>

参考：[什么是volatile关键字?](https://mp.weixin.qq.com/s?__biz=MzIxMjE5MTE1Nw==&mid=2653192450&idx=2&sn=ad95717051c0c4af83923b736a5bc637&chksm=8c99f3d8bbee7aceb123e4f6aa9a220630b5aa17743ba812d82308bfb6a8ed8303bdd181f144&scene=21#wechat_redirect)

### Synchronized
Synchronize是java中重量级的锁机制，可以修饰方法、代码块来实现同步机制：<br/>
#### 实现原理
Java对象头：JVM中用两个字节来存储对象头，主要为Mark Word 结构，包含了锁标记、hashcode、分代年龄以及GC信息。<br/>
Monitor(监视器锁)：当对象头中的锁标记被标记后，其指针指向Monitor对象的起始位置。每一个对象都关联着一个Monitor,Monitor可以和对象一起创建和销毁，或者线程获取对象锁时自动生成。当Monitor被一个线程持有后就处于锁定状态，其内部是通过ObjectMonitor实现。ObjectMonitor中有两个队列，_WaitSet 和 _EntryList，用来保存ObjectWaiter对象列表( 每个等待锁的线程都会被封装成ObjectWaiter对象)，_owner指向持有ObjectMonitor对象的线程，当多个线程同时访问一段同步代码时，首先会进入 _EntryList 集合，当线程获取到对象的monitor 后进入 _Owner 区域并把monitor中的owner变量设置为当前线程同时monitor中的计数器count加1，若线程调用 wait() 方法，将释放当前持有的monitor，owner变量恢复为null，count自减1，同时该线程进入 WaitSe t集合中等待被唤醒。若当前线程执行完毕也将释放monitor(锁)并复位变量的值，以便其他线程进入获取monitor(锁)。<br/>
* 修饰方法块：当方法块被Synchronized修饰后，在方法的字节码中会加入monitorenter 和 monitorexit 指令。monitorenter指令指向同步代码块的开始位置，monitorexit指令则指明同步代码块的结束位置，当执行monitorenter指令时，当前线程将试图获取 objectref(即对象锁) 所对应的 monitor 的持有权，当 objectref 的 monitor 的进入计数器为 0，那线程可以成功取得 monitor，并将计数器值设置为 1，取锁成功。如果当前线程已经拥有 objectref 的 monitor 的持有权，那它可以重入这个 monitor (关于重入性稍后会分析)，重入时计数器的值也会加 1。倘若其他线程已经拥有 objectref 的 monitor 的所有权，那当前线程将被阻塞，直到正在执行线程执行完毕，即monitorexit指令被执行，执行线程将释放 monitor(锁)并设置计数器值为0 ，其他线程将有机会持有 monitor 。值得注意的是编译器将会确保无论方法通过何种方式完成，方法中调用过的每条 monitorenter 指令都有执行其对应 monitorexit 指令，而无论这个方法是正常结束还是异常结束。为了保证在方法异常完成时 monitorenter 和 monitorexit 指令依然可以正确配对执行，编译器会自动产生一个异常处理器，这个异常处理器声明可处理所有的异常，它的目的就是用来执行 monitorexit 指令。字节码中多了一个monitorexit指令，它就是异常结束时被执行的释放monitor 的指令。
* 修饰方法：方法级的同步是隐式，即无需通过字节码指令来控制的，它实现在方法调用和返回操作之中。JVM可以从方法常量池中的方法表结构(method_info Structure) 中的 ACC_SYNCHRONIZED 访问标志区分一个方法是否同步方法。当方法调用时，调用指令将会 检查方法的 ACC_SYNCHRONIZED 访问标志是否被设置，如果设置了，执行线程将先持有monitor（虚拟机规范中用的是管程一词）， 然后再执行方法，最后再方法完成(无论是正常完成还是非正常完成)时释放monitor。在方法执行期间，执行线程持有了monitor，其他任何线程都无法再获得同一个monitor。如果一个同步方法执行期间抛 出了异常，并且在方法内部无法处理此异常，那这个同步方法所持有的monitor将在异常抛到同步方法之外时自动释放。<br/>

#### 锁的优化
为了减少获得锁和释放锁带来的性能消耗，引入了“偏向锁”和“轻量级锁”，锁一共有4种状态，级别从低到高依次是：无锁状态、偏向锁状态、轻量级锁状
态和重量级锁状态，这几个状态会随着竞争情况逐渐升级。锁可以升级但不能降级，意味着偏向锁升级成轻量级锁后不能降级成偏向锁。<br/>
* 偏向锁：经过这么多年java的使用过程中发现，很多情况下并不存在锁不仅不存在多线程竞争，而且总是由同一线程多次获得，为了让线程获得锁的代价更低而引入了偏向锁。当一个线程访问同步块并获取锁时，会在对象头和栈帧中的锁记录里存储锁偏向的线程ID，以后该线程在进入和退出同步块时不需要进行CAS操作来加锁和解锁，只需简单地测试一下对象头的Mark Word里是否存储着指向当前线程的偏向锁。如果测试成功，表示线程已经获得了锁。如果测试失败，则需要再测试一下Mark Word中偏向锁的标识是否设置成1（表示当前是偏向锁）：如果设置了，则尝试使用CAS将对象头的偏向锁指向当前线程，如果没有设置，则使用CAS竞争锁。如果此时线程获取锁失败，表明存在锁的竞争，那么系统就会进行锁升级，升级为轻量级锁。
* 轻量级锁：线程在执行同步块之前，JVM会先在当前线程的栈桢中创建用于存储锁记录的空间，并将对象头中的Mark Word复制到锁记录中，官方称为Displaced Mark Word。然后线程尝试使用CAS将对象头中的Mark Word替换为指向锁记录的指针。如果成功，当前线程获得锁，如果失败，表示其他线程竞争锁，当前线程便尝试使用自旋来获取锁。当代码块执行完毕后，轻量级解锁，会使用原子的CAS操作将Displaced Mark Word替换回到对象头，如果成
功，则表示没有竞争发生。如果失败，表示当前锁存在竞争，其他线程在通过自旋获得锁失败后已经将锁升级为重量级锁，也就是重量级锁Synchronized。<br/>
#### 注意点
 * synchronized修饰普通同步方法，锁是当前实例对象，修饰静态方法，锁是当前类的class对象，修饰方法块时，锁是Synchonized括号里配置的对象；
 * synchronized关键字不能修饰int、double等基本数据类型；
 * synchronized关键字最好不要修饰String、Integer等基本数据对象类型，因为如果基本数据对象类型的值发生改变的话，原先加的锁可能会丢失；
 * synchronized关键字修饰对象时，如果对象的属性值发生改变（对象本身发生改变例外）不会影响锁的稳定；
 
### 原子操作
所谓“原子”，是表示不能被进一步分割的粒子，而原子操作则表示一个或者一系列不能被中断的操作。
#### 处理器保证原子性：
* 总线锁保证原子性：使用处理器提供的一个LOCK＃信号，当一个处理器在总线上输出此信号时，其他处理器的请求将被阻塞住，那么该处理器可以独占共享内存。
* 缓存锁保证原子性：在同一时刻，我们只需保证对某个内存地址的操作是原子性即可，但总线锁定把CPU和内存之间的通信锁住了，这使得锁定期间，其他处理器不能操作其他内存地址的数据，所以总线锁定的开销比较大，因此使用缓存锁定代替总线锁定来进行优化。频繁使用的内存会缓存在处理器L1、L2和L3高速缓存里，那么原子操作就可以直接在处理器内部缓存中进行，并不需要声明总线锁。所谓“缓存锁定”是指内存区域如果被缓存在处理器的缓存
行中，并且在Lock操作期间被锁定，那么当它执行锁操作回写到内存时，处理器不在总线上声言LOCK＃信号，而是修改内部的内存地址，并允许它的缓存一致性机制来保证操作的原子性，因为缓存一致性机制会阻止同时修改由两个以上处理器缓存的内存区域数据，当其他处理器回写已被锁定的缓存行的数据时，会使缓存行无效。<br/>
处理器提供了很多Lock前缀的指令来实现内存加锁。例如，位测试和修改指令：BTS、BTR、BTC；交换指令XADD、CMPXCHG，以及其他一些操作数和逻辑指
令（如ADD、OR）等，被这些指令操作的内存区域就会加锁，导致其他处理器不能同时访问它。
#### Java保证原子性：
在Java中可以通过锁和循环CAS的方式来实现原子操作。<br/>
循环CAS：比较并交换，该操作需要两个值，一个旧值(期望操作前的值)，一个新值，在操作期间先比较旧值有没有有没有改变，如果没有则变化成新值，否则不交换。VM中的CAS操作是利用了处理器提供的CMPXCHG指令实现的。自旋CAS实现的基本思路就是循环进行CAS操作直到成功为止。比如使用循环CAS实现线程安全计数器：<br/>
```java
AtomicInteger atomicI = new AtomicInteger(0);
 
private void safeCount() {
   for (;;) {
     int i = atomicI.get();
     boolean suc = atomicI.compareAndSet(i, ++i);
     if (suc) {
        break;
     }
   }
}
```
CAS实现原子操作的三大问题：<br/>
* ABA问题：因为CAS需要在操作值的时候，检查值有没有发生变化，如果没有发生变化则更新，但是如果一个值原来是A，变成了B，又变成了A，那么使用CAS进行检查时会发现它的值没有发生变化，但是实际上却变化了。ABA问题的解决思路就是使用版本号。在变量前面追加上版本号，每次变量更新的时候把版本号加1，那么A→B→A就会变成1A→2B→3A。从Java 1.5开始，JDK的Atomic包里提供了一个类AtomicStampedReference来解决ABA问题。这个类的compareAndSet方法的作用是首先检查当前引用是否等于预期引用，并且检查当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。
* 循环时间长开销大。自旋CAS如果长时间不成功，会给CPU带来非常大的执行开销。如果JVM能支持处理器提供的pause指令，那么效率会有一定的提升。pause指令有两个作用：第一，它可以延迟流水线执行指令（de-pipeline），使CPU不会消耗过多的执行资源，延迟的时间取决于具体实现的版本，在一些处理器上延迟时间是零；第二，它可以避免在退出循环的时候因内存顺序冲突（Memory Order Violation）而引起CPU流水线被清空（CPU Pipeline Flush），从而提高CPU的执行效率。
* 只能保证一个共享变量的原子操作。当对一个共享变量执行操作时，我们可以使用循环CAS的方式来保证原子操作，但是对多个共享变量操作时，循环CAS就无法保证操作的原子性，这个时候就可以用锁。<br/>

有意思的是除了偏向锁，JVM实现锁的方式都用了循环CAS，即当一个线程想进入同步块的时候使用循环CAS的方式来获取锁，当它退出同步块的时候使用循环CAS释放锁。

### 写一个死锁
```java
 public static void testLock() {
        String A = "a";
        String B = "b";
        
        Thread t1 = new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized(A){
                    try {
                        Thread.currentThread().sleep(2000);
                        
                        synchronized(B){
                            System.out.println("T1.run()");
                        }
                    } catch (InterruptedException ex) {
                        Logger.getLogger(JavaApplication1.class.getName()).log(Level.SEVERE, null, ex);
                    }
                    
                }
            }
        });
        
        Thread t2 = new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized(B){
                    
                    synchronized(A){
                            System.out.println("T2.run()");
                    }
                }
            }
        });
        
        t1.start();
        t2.start();
    }
```
 ### synchronized关键字实现两个线程交替打印奇偶数
 ```java
 static class NumberTask implements Runnable{
        static int value = 0;
        @Override
        public void run() {
            synchronized (NumberTask.class){
                while (value < 100){
                    System.out.println("thread:" + Thread.currentThread().getName() + "value:" + ++value);
                    NumberTask.class.notify();
                    try {
                        NumberTask.class.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    
    new Thread(new NumberTask(),"奇数").start();
    new Thread(new NumberTask(),"偶数").start();
 ```
 
 ### lock三个线程依次交替打印1到100
 
 ```java
 static class LockThread extends Thread{
        static Lock lock = new ReentrantLock();

        int count;
        int id;
        int maxV;
        static int value = 0;
        public LockThread(int id,int count,int maxV){
            this.id = id;
            this.count = count;
            this.maxV = maxV;
        }
        @Override
        public void run() {
            while (value <= maxV){
                if (value % count == id){
                    if (lock.tryLock()){
                        System.out.println("thread:" + id + " value:" + value++);
                        lock.unlock();
                    }
                }
            }
        }
    }
    
    for (int i =0;i < 3;i++){
            new LockThread(i,3,100).start();
    }
 ```
 
 ### java异常处理
 关系图：<br/>
![avatar](/image/error.png)<br/>
Exception 和 Error 都是继承了 Throwable 类，在 Java 中只有 Throwable类型的实例才可以被抛出（throw）或者捕获（catch），它是异常处理机制的基本组成类型。<br/>
* Error 是指在正常情况下，不大可能出现的情况，java 运行时系统的内部错误和资源耗尽错误,绝大部分的 Error 都会导致程序（比如 JVM 自身）处于非正常的、不可恢复状态。既然是非正常情况，所以不便于也不需要捕获，常见的比如 OutOfMemoryError 、StackOverflowError之类，都是 Error的子类。
* Exception 是程序正常运行中，可以预料的意外情况，可能并且应该被捕获，进行相应处理。Exception 又分为可检查（checked）异常和不检查（unchecked）异常，可检查异常在源代码里必须显式地进行捕获处理，比如IOException、InterruptedException等这是编译期检查的一部分。不检查异常就是所谓的运行时异常，类似 NullPointerException、ClassCastException之类，通常是可以编码避免的逻辑错误，具体根据需要来判断是否需要捕获，并不会在编译期强制要求。<br/>

在代码中，通常采用try-catch-finally来捕捉异常，throw字段抛出异常，throws字段来定义我们编写的方法在被调用时抛出何种类型的异常。值得注意的是，try-catch 代码段会产生额外的性能开销，或者换个角度说，它往往会影响 JVM 对代码进行优化，所以建议仅捕获有必要的代码段，尽量不要一个大的 try 包住整段的代码；与此同时，利用异常控制代码流程，也不是一个好主意，远比我们通常意义上的条件语句（if/else、switch）要低效。
 
 ### java是值传递还是引用传递？
 https://www.zhihu.com/question/31203609

