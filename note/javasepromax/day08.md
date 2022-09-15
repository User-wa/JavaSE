# 日志框架

## 日志技术的概述

* 日志就相当于日记。当出现bug时，需要日志来找到在哪里。

* ![image-20220820151302112](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820151302112.png)

## 日志技术体系

* ![image-20220820151435715](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820151435715.png)

## Logback概述

* ![image-20220820151511863](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820151511863.png)

* 
* ![image-20220820151547544](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820151547544.png)

## Logback初步配置

* ![image-20220820165820067](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820165820067.png)

* ```java
  //创建日志对象，代表日志技术
      public static final Logger LOGGER = LoggerFactory.getLogger("Test.class");//表示当前类
  ```

## Logback配置详解：输出位置，格式设置

* ![image-20220820170303605](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820170303605.png)

## Logback配置详解：日志级别设置

* ![image-20220820172751415](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820172751415.png)

# 	项目

![image-20220822104057241](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220822104057241.png)

![image-20220822104144766](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220822104144766.png)

![image-20220822102833547](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220822102833547.png)

*

![image-20220822104004964](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220822104004964.png)

**

![image-20220822112309376](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220822112309376.png)

**

![image-20220822112920498](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220822112920498.png)

**

![image-20220822203945619](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220822203945619.png)

**

![image-20220822204114924](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220822204114924.png)

要用到子类的方法，需要将父类的类型转换为子类的类型。

得到对象要用容器存起来，而不是像我一样一直链式结构！！！！这样会导致程序可读性差！！！查bug很累！

**

![image-20220822205420567](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220822205420567.png)

根据影片名字查影片！删除和修改影片信息都需要用到，所以定义成一个方法，方便调用。！！

**

![image-20220822213234338](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220822213234338.png)

BigDecimal为类名，调取它的静态方法可以不创建对象。

存放父类，比存放具体的值更优秀！！因为返回一个对象，比返回一个值更好，可以通过对象地址来调用其他的属性。

## 个人总结

* 码前

  * 需要将日志文件配好，通过 logger LOGGER = logger.factory(类名)创建

  * 将实体类和父类，接口，放在bean包下

  * 将要具体run起来的放在run包下

  * 对于一个项目，要事先分析其业务需要，需要定义什么来装对象，主要的流程又是那些要搞清楚。

* 码中

  * 通过使用继承的方式，将多个类中的相同属性或方法，用一个父类来存取。并采用多态的形式，来储存子类对象。同一个父类，只创建父类集合，就可以存放子类对象，再通过instanceof来判断类型。同时调用有参父类构造器，会初始化父类所有属性！

  * 对于要事先存的数据，使用静态代码块

  * ```java
     public static final Map<User, List<Movie>> ALL_MOVIES = new HashMap<>();
     public static final Map<String, List<Movie>> ALL_MOVIES = new HashMap<>();
    ```

  * 将引用类型final化，只是final它本身的地址，并不影响其添加数据。变成常量记得大写。通过将map的key类型定为User父类，就可以调用其具体的属性，用对象，而不是String。

  * ```java
    SYS_SC.nextLine();
    ```

  * 通过nextLine来接数据，就不会出现类型接收错误，之后在采用包装类的方式，对字符串进行解码，而且后面也要用nextLine来接，不然会出现缓冲区异常。

  * return可以将foreach结束掉

  * ArrayList集合的get方法没有get到返回null

  * ```java
     List<Movie> movies = ALL_MOVIES.get(nowUser);
    ```

  * 将得到的值重新赋值，有助于代码的理解。

