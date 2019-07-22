## 记录java相关！

### 1、在java中，public、privite、protected修饰符
在java中，public、privite、protected三种修饰符用来修饰类以及类的成员变量和方法。</br>
修饰类的成员变量以及方法时：privite，变量和方法只能在类中可见，对子类以及其他包中的类是不可见的，方法不可见，那么子类也无法对父类中privite修饰的方法进行重写。public，对子类以及其他包里的类都可见。protected，子类以及同一个包中的类均可访问。可以这么理解，只要方法和变量不是private修饰，那么就对其子类可见。</br>
修饰类时：privite修饰类，那么类中的所有数据(变量，方法)都将被隐藏，一般没有太大意义，因为我们定义一个类，就是用来创建对象并调用的，弄成隐藏，还有什么意义（除了内部类，一般的类也不允许这么修饰）。public修饰类，该类对所有类可见。protected修饰类，该类只能对同包中的类可见。

* 默认情况下，作用域相当于protected；
* 类的权限设定会约束成员的权限，这应该很好理解。

### 2、final修饰符

final字段可修饰类、方法以及变量。

* 对于一个final变量，如果是基本数据类型的变量，那么其值在初始化之后是不可以改变的。一个既是static又是final修饰的变量只占据一段不能改变的内存；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。
* 当用final修饰一个类时，表明这个类不能被继承。final类中的所有成员方法都会被隐式地指定为final方法,不能被子类重写。<br/>
* 使用final方法的原因有两个。第一个原因是把方法锁定，以防任何继承类修改它的含义；第二个原因是效率。在早期的Java实现版本中，会将final方法转为内嵌调用。但是如果方法过于庞大，可能看不到内嵌调用带来的任何性能提升（现在的Java版本已经不需要使用final方法进行这些优化了）。类中所有的private方法都隐式地指定为final。

### 3、static修饰符

通常情况下，会出现多个类在内存区域共享一个数据，这是我们就通过static来修饰这个数据。static可以修饰类，方法、变量以及代码块，java虚拟机会为static修饰的变量等分配唯一的内存区域，且周期跟随进程。使用static需要注意以下几点：

* 静态成员同样遵循public、private、protected修饰符的约束；
* 静态方法中不可以使用this字段；
* 静态方法不可以直接调用非静态方法；
* 方法中的局部变量不能声明为static，即使是静态方法也不行。

### 4、封装、继承、多态
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

### 5、内部类
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
### 6、泛型
### 7、枚举
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
### 8、多线程
### 9、类加载过程
### 10、String、StringBuffer和StringBuilder的区别。

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

### 11、ArrayList 与LinkedList以及Vector的区别
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
