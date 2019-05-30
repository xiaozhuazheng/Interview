## 记录java以及计算机网络相关！

### 在java中，public、privite、protected修饰符
在java中，public、privite、protected三种修饰符用来修饰类以及类的成员变量和方法。</br>
修饰类的成员变量以及方法时：privite，变量和方法只能在类中可见，对子类以及其他包中的类是不可见的，方法不可见，那么子类也无法对父类中privite修饰的方法进行重写。public，对子类以及其他包里的类都可见。protected，子类以及同一个包中的类均可访问。可以这么理解，只要方法和变量不是private修饰，那么就对其子类可见。</br>
修饰类时：privite修饰类，那么类中的所有数据(变量，方法)都将被隐藏，一般没有太大意义，因为我们定义一个类，就是用来创建对象并调用的，弄成隐藏，还有什么意义（除了内部类，一般的类也不允许这么修饰）。public修饰类，该类对所有类可见。protected修饰类，该类只能对同包中的类可见。

* 默认情况下，作用域相当于protected；
* 类的权限设定会约束成员的权限，这因该很好理解。

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

### 内部类
### 封装、继承、多态
### 泛型
### 枚举
### 多线程
### 类加载过程