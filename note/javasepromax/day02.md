# 包，权限修饰符

## 包

* ![image-20220820092736759](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820092736759.png)

* 调用不同包下的同名类，默认导入一个包，另一个类要带包名访问

## 权限修饰符

* ![image-20220820092815940](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820092815940.png)

* 注意：不同包的子类！

* ```pava
  public class Z extends A{//子类对象调用父类的protected方法，而不是创建父类对象。
  	Z Z1 = new Z;
  	Z.方法名
  }
  ```

# final

*  

![image-20220820092918413](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820092918413.png)

* ```java
  final Student stu = new Student();
  final修饰的引用变量地址值不发生改变，但其指向的内容可以发生改变
  ```

# 常量

## 常量的概述和基本作用

* ![image-20220820092958635](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820092958635.png)

* 使用public static final 修饰的成员变量，可以用于做系统的配置信息，方便程序的维护。与字面量的性能是一致的

## 常量做信息标志和分类

* ![image-20220820093039614](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820093039614.png)

* ```java
  public static final int UP = 1;
  move(UP);
  ......
  case(up):
  	break;
  ```

* 常量做信息标志，能更好的实现软编码模式，看起来更优雅

# 枚举

## 枚举的概述

* 枚举是java中的一种特殊类型
* 枚举的作用：是为了做信息的标志和信息的分类

* 定义枚举类的格式：

  * ```java
    public enum enumDemo{
        SPRING, SUMMER, AUTUMN, WINTER;//第一行罗列枚举类实例的名称。
    }
    ```

* ![image-20220820093128346](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820093128346.png)

## 枚举的使用场景演示

* 表示具体数据值，用常量。
* 信息分类的话，用枚举。

# 抽象类

## 抽象类的概念

* abstract，修饰类，成员方法

* ```java
  public abstract class abstractDemo{
      public abstract void run();
  }
  ```

* 只能声明方法签名，不能声明方法体

* 有抽象方法必须类也是抽象类

* **初步使用场景**
  
* 父类知道子类一定要调用这个方法，但是每一个子类调用这个行为的具体实现不一样
  
* **抽象类基本作用是啥？**

  * 作为父类，用来被继承

* **需要注意的问题**

  * 一个类如果继承了抽象类，这个类必须重写抽象类的全部抽象方法，否则这个类也必须定义成抽象类

## 抽象类的特征，注意事项小结

*  ![image-20220820093349561](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820093349561.png)

* 是一个类，所以类的成员抽象类中都可以有
* 抽象类中不一定有抽象方法
* 抽象类和抽象方法
* 不能创建对象

### final和abstract关系

* ![image-20220820093310548](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820093310548.png)

## 抽象类的应用知识：模板方法模式

* ![image-20220820093450031](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820093450031.png)

* ![image-20220820093527747](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820093527747.png)

* 模板方法有final修饰会显得更专业。因为模板方法不需要被子类重写，被重写了模板就不叫模板了

* ```java
  package come.halou.Test1;
  
  public abstract class Student {
      public final void write(){
          System.out.println("hello");
          System.out.println(writeMain());
          System.out.println("world");
      }
      public abstract String writeMain();
  }
  
  package come.halou.Test1;
  
  public class StudentChild extends Student{
  
      @Override
      public String writeMain() {
          String str1 = "我好帅";
          return str1;
      }
  }
  
  ```

# 接口

## 接口的概述和特点

* ```java
  public interface interfaceDemo{
     	常量和抽象方法
  }
  ```

* 

* 接口是一种规范，需要公开让大家都知道
* 由于要公开，所以在定义接口时，jdk8之前的抽象方法和常量都分别可以省略public abstract ，public static final；

## 接口的基本使用：被实现

* ![image-20220820093724517](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820093724517.png)

* 接口是一种规范，实现这种规范的类叫做实现类。

## 接口与接口的关系

* ![image-20220820095409037](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820095409037.png)

* 接口的多继承是为了整合多个接口为一个接口，便于子类的实现 

## JDK8开始接口新增方法（接口可以加方法体）

* ![image-20220820095653273](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820095653273.png)

## 使用接口的注意事项

![image-20220820102623841](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820102623841.png)

* 2，是接口名.方法名才能调用。3，就近。