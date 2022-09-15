# static静态关键字

## static是什么，修饰成员变量的用法

* 成员变量的分类和访问是怎样的？
  * 静态成员变量（有static修饰，属于类，加载一次，可以被共享访问）
    * 类名.静态成员变量（推荐）
    * 对象.静态成员变量（不推荐）
  * 实例成员变量（无static修饰符，属于对象）
    * 对象.实例成员变量
* 静态成员变量：表示在线人数等需要共享的信息
* 实例成员变量：属于每一个对象，且每一个对象信息都不同。

## static修饰成员变量的内存原理

* ![image-20220820083833900](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820083833900.png)

## static修饰成员方法的基本用法

* ![image-20220820090430588](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820090430588.png)

* 分为静态成员方法（执行一个通用功能为目的的），实例成员方法（需要访问实例成员的，就要设计）。

## static修饰成员方法的内存原理

* ![image-20220820090505222](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820090505222.png)

* 静态成员方法在形成.class文件时已经生成,所以通过类名调用可以得到，而实例成员方法当创建对象时才会生成，实例方法的地址传入方法区中

## static注意事项

* 静态方法只能访问静态的成员，不可以直接访问实例成员
* 实例方法可以访问静态的成员，也可以访问实例成员
* 静态方法中是不能出现this关键字的

# static应用知识：工具类

* 工具类内部是一些静态方法（完成通用功能），每个方法完成一个功能
* 一次编写，处处可用，可以提高代码的复用性
* 需要将工具类中的构造器私有化

# static应用知识：代码块

* ![image-20220820090615130](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820090615130.png)

* 静态代码块和类是一个级别的，当执行类时，会自动调用静态代码块中的内容。
* 构造代码块是在构造对象时，即进入构造器前运行。

# static应用知识：单例设计模式

![image-20220820090737206](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820090737206.png)

## 饿汉单例模式

![image-20220820090719873](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820090719873.png)

## 懒汉单例模式

* ![image-20220820090817205](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820090817205.png)

# 继承

## 继承的概述，使用继承的好处

* ![image-20220820090953991](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820090953991.png)

* ![image-20220820091254869](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820091254869.png)

* 得到父类的属性和行为。

* 提高代码复用性，子类共有的代码放在父类中

## 继承的设计规范，内存运行原理

* ![image-20220820091740808](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820091740808.png)

* 继承的内存运行原理，创建子类对象时，会同时在堆内存中开辟父类的空间，返回的还是一个地址
* ![image-20220820091409471](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820091409471.png)

* 共有的特征放在父类中，独有的就放在子类自己里面

## 继承的特点

* 子类可以继承父类的属性和行为，但不能继承父类的构造器
* java是单继承模式：一个类只能继承一个直接父类
* java不支持多继承，但是支持多层继承
* java中所有的类都是object类的子类 

## 继承后：成员变量，成员方法的访问特点

* ![image-20220820091951215](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820091951215.png)

* 访问成员采用就近原则。先找局部，再找子类，再找父类，父类没有就报错。

## 继承后：方法重写

![image-20220820092209638](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820092209638.png)

* 子类不能重写父类的静态方法，如果重写会报错

* 申明不变，重新实现

## 继承后，子类构造器的特点

* ![image-20220820092344099](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820092344099.png)

* 都会自动调用父类的无参构造器（super（）），再调用本类的。（需要将父类的数据初始化）

## 继承后，子类构造器访问父类有参构造器

* ![image-20220820092508972](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820092508972.png)

* ```java
  public teacher(String name, double age){//来初始化父类中的数据
      super(name, age);
  }
  ```

* 无论怎样，子类构造器都需要调用父类构造器，使得父类的数据初始化

## this（）和super（）使用注意点

* this（）调用兄弟构造器

* ```java
  public Dog(String name){
      this(name, 18);
  }
  public Dog(String name, int age){
      this.name = name;
      this.age = age;
  }
  ```

  

* ![image-20220820092611940](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820092611940.png)

* this和super（默认）只能有一个放在构造器第一行，一定要调用父类构造器。
* 当子类构造器调用子类兄弟构造器时，兄弟构造器会自动调用父类无参构造器。

