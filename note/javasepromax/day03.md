# 多态

## 多态的概述，多态的形式

* ![image-20220820102818823](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820102818823.png)

* 执行同一行为，因为会表现出不同的行为特征，所以父类中应定义abstract方法。抽象类，子类对象会重写。

* **编译时方法行为表现的是父类中的方法，运行时方法行为表现的是[子类](https://so.csdn.net/so/search?q=子类&spm=1001.2101.3001.7020)中重写该方法的行为特征** 

* ```java
  Animal A = new Dog();//多态，需要有方法重写
  
  ```

## 多态的优势

* 在多态的情况下，右边对象可以实现解耦合，便于扩展和维护。
* ![image-20220820102951420](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820102951420.png)

* 应为方法编译看左边，运行看右边
* 定义方法时，使用父类型作为参数，该方法就可以接收父类的一切子类对象，体现出多态的扩展性与便利

* **问题**
  * 多态下不能使用子类的独有功能。

## 多态下引用数据类型的类型转换

* ![image-20220820103025375](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820103025375.png)

* ```java
  自动类型转换：Animal a = new Dog();
  强制类型转换：Dog a1 = (Dog)a
      如果是 Tortoise a1 = (Tortoise)a 编译不会出错，而运行因为转换后不是真实类型而报错
  ```

* 通过instanceof来判断转换后的类型和对象真实类型是否一致，不一致会报ClassCastException.

* ```java
  a instanceof Dog 正确的话，返回true
  ```


# 内部类

## 内部类的概述

* 内部是相当于外部而言的，比如汽车类中有发动机类，是定义在一个类里面的类，里面的类可以理解为（寄生），外部类为（寄主）

* ![image-20220820103111290](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820103111290.png)

* 分为
  * 静态内部类
  * 成员内部类(非静态内部类)
  * 局部内部类
  * 匿名内部类（重点）

## 静态内部类

* ![image-20220820103212215](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820103212215.png)

* ```java
  Outer.Inner in = new Outer.Inner;
  ```

* 

## 成员内部类

* ![image-20220820103819525](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820103819525.png)

* 成员内部类与静态内部类的使用：一般来说，都是要先有汽车类，再有发动机类,所以可能成员内部类用的多些

* ```java
  Outer.Inner in = new Outer().new Inner();
  ```

* **注意**：

  * ![image-20220820103956780](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820103956780.png)

  * ```java
    System.out.println(Outer.this.number);
    ```

## 局部内部类

* ![image-20220820104012070](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820104012070.png)

## 匿名内部类

* ![image-20220820104131228](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820104131228.png)

* ```java
  Animal a = new Dog();
  
  Animal a = new Animal(){//产生一个没有名字的内部类对象。其对象类型相当于当前new对象的子类。
      public void work(){
          
      }
  }
  a.work
  ```

## 匿名内部类的常见形式

* ![image-20220820104318593](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820104318593.png)

* ```java
  goSwimming(new Animal(){
      public void swim(){
          
      }
  })
  //直接作为方法的实际参数进行传输。    
  ```

# 常用API

## Object

### toString

* ![image-20220820104515891](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820104515891.png)
* 默认返回地址
* 让子类重写，子类重写后返回子类对象内容

### equals

* ![image-20220820104658322](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820104658322.png)

* 默认返回Boolean类型，比较两对象地址是否相同，但开发中是比较两子类对象是否内容相等，需要重写方法

## Objects

* ![image-20220820104856695](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820104856695.png)

* ![image-20220820104944328](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820104944328.png)

* ![image-20220820105024006](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105024006.png)

* 当主调为null时，如果用str1.equals()  str1为null会报出空值异常，而是用objects.equals()就不会，更安全。

## StringBuilder

* ![image-20220820105104730](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105104730.png)
* ![image-20220820105122673](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105122673.png)

* ```java
  支持链式 ： StringBuilder sb = new StringBuilder();
  			sb.append().append().append();
  因为add()返回的还是StringBuilder 对象
  ```

* StringBuilder只是拼接，修改字符串的手段，最终的结果还是要返回String类型

* ```java
  String s = sb.toString();
  ```

#### 原理图

* ![image-20220820105219991](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105219991.png)

* 会把前面的对象都丢了。

* ![image-20220820105239467](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105239467.png)

* ![image-20220820105319084](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105319084.png)

## Math类

* ![image-20220820105347996](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105347996.png)

## System类

* ![image-20220820105408618](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105408618.png)

* exit()慎用，currenttimemillis（）可以看程序的性能

## BigDecimal

* ![image-20220820105515098](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105515098.png)

* ![image-20220820105537890](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105537890.png)
* 通过方法调用，来创建BigDecimal对象

* 和StringBuilder差不多，进行浮点型的加减乘除 ，但只是一种手段，真正返回时通过doubleValue（）方法返回double类型数据。
* 当3 /10时，由于本来就是无限循环小数，所以无法精度运算。需要用到divide（）参数方法。