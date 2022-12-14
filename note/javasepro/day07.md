# 面向对象

## 设计对象并使用

### 设计类，创建对象并使用

* 类是设计图，是模板，是东西，可以拿来用的东西
* 对象是类的实例
* ![image-20220820081507128](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820081507128.png)

* 创建对象
  * Student student = new Student()；
  * 类名 对象名 = new 类名()

* 访问对象属性，方法.
  * 实例名 .成员属性, 实例名.成员方法()

### 定义类的注意事项

* 首字母大写，用驼峰形式
* 一个类中只能存在一个public类 且名字与文件名相同，虽然可以定义多个类，但建议创建新的类
* 成员变量的格式 ：修饰符 数据类型 变量名称 = 初始化值。一般都用默认值代替，而并非赋值。

## 对象内存图

### 多个对象的内存图

* ![image-20220820081651622](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820081651622.png)

* 对象放在堆内存中
* 对象名存储的是对象在堆内存的地址
* 成员变量是放在堆内存中

### 两个变量指向同一个对象内存图

* ![image-20220820081747823](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820081747823.png)

#### 垃圾回收机制

* 当**堆内存中的对象**，没有被任何变量引用时，就会被判定为内存中的“垃圾”。

## 购物车

* 购物车中的每个商品可以看成什么，需要先做什么准备？
  * 可以看出一个一个对象
  * 定义商品类：Goods，后期创建对象代表一个商品信息
* 购物车对象用什么表示的，可以用来做什么？
  * 购物车可以是商品类型的数组对象表示，可以用于存放商品对象

## 构造器

* 作用
  * 初始化类的对象，并返回对象的地址
* 构造器有哪几种？各自的作用
  * 无参数构造器：初始化对象时，成员变量的数据均采用默认值
  * **有参数构造器：在初始化对象时，可以接受参数为对象赋值**

* 注意事项
  * ·无参数构造器默认有
  * 一旦定义了有参数构造器，要想再次调用无参数构造器，就需要自己写了

## this关键字

* 概念
  * 出现在构造器和成员方法中，代表当前对象的地址
* 可以做什么？
  * 可用于指定访问当前对象的成员

## 封装

### 封装思想概述

* 什么是封装？
  * 告诉我们怎样设计属性和方法
  * 原则：对象代表什么，就得封装对应的数据，并提供数据对应的行为
* 理解封装思想有什么好处？
  * 让编程更简单，有什么事情，找对象，调方法就行

### 如何更好的封装

* 一般会把成员变量通过private 修饰符 隐藏，在类的外部不能访问
* 提供public 修饰的getter 和 setter方法暴露其取值和赋值

## 标准JavaBean

* ![image-20220820081859267](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820081859267.png))
* 实体类：学生类，汽车类等。。。而不是test类。。
* 创建对象，封装数据的

## 成员变量和局部变量的区别

* ![image-20220820081941628](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820081941628.png)

## Movie[] movies = new Movie(); 

## movie m = movies[0];

* ![image-20220820082039539](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820082039539.png)

