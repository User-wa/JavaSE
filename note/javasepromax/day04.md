# 日期与时间

## Date

* 通过Date类中的getTime方法/System.currenttime......方法获取时间毫秒值，通过有参构造，或者setTime方法将毫秒值转换成当前时间。

* ![image-20220820105653320](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105653320.png)

## SimpleDataFormat

* ![image-20220820105739303](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105739303.png)

* ![image-20220820105814189](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105814189.png)

* ![image-20220820105840892](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105840892.png)

* ![image-20220820105944566](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820105944566.png)
* 格式化format可以格式化date对象或者毫秒值传出的是String类型
* System.current .... 返回的是当前时间.
* parse解析，创建SimpleDateFormat对象时要传入相同格式。

## Calendar

* ![image-20220820110156304](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820110156304.png)

* 得到日历对象

* ```java
  Calendar ca = Calendar.getInstance();
  ```

* 

  ​	

# JDK8新增日期类

## LocalTime LocalDate LocalDateTime

* 得到月份，年份。。。，或进行修改更简单 他们都是不可变对象，会返回一个新的对象。LocalDateTime 综合了 localTime,localDate里面的方法
* ![image-20220820110339587](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820110339587.png)
* ![image-20220820110404928](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820110404928.png)
* LocalDateTime转换
* ![image-20220820110702200](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820110702200.png)

## Instant

* 可以表示时间戳
* ![image-20220820110735048](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820110735048.png)

## DateTimeFormatter(时间格式器)

![image-20220820110758497](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820110758497.png)

## Duration/Period(算时间间隔)

![image-20220820111401861](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820111401861.png)

* ![image-20220820111415197](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820111415197.png)
* Duration 计算的是两个时间的间隔
* Period 用于计算两个日期的间隔

## ChronoUnit

![image-20220820111441581](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820111441581.png)

* 单个时间单位内测量一段时间

# 包装类

* 概念
  * 基本数据类型对应的引用类型：int --> Integer char -->Character 其他都是首字母大写
  * 实现了一切皆对象
  * 集合，泛型都只支持引用类型变量，不支持基本数据类型，所以需要使用包装类
* 功能
  * 支持把基本类型转换成字符串类型（没多大用）
  
  * 可以把字符串类型的数值转换成真实的数据类型(很有用)
  
  * ```java
    String s1 = "1234";
            int a4 = Integer.valueOf(s1);
            System.out.println(a4 + 'a');
    ```
  
  * 

# 正则表达式

* 概念：用于校验数据是否规范
* 特点：代码简化，字符串.matches()方法
* API 中搜索Pattern查看

## 匹配规则

* ![image-20220820111525348](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820111525348.png)

## 正则表达式在方法中的使用

* ![image-20220820111635144](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820111635144.png)

## 正则表达式支持爬取信息

* ![image-20220820111619589](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820111619589.png)

# Arrays类（数组工具类）

## Arrays类概述，常用功能

* ![image-20220820111723439](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820111723439.png)

## Arrays类对于Comparator比较器的支持

* ![image-20220820111754248](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820111754248.png)

* 使用比较对象自定义

* ```java
  public class ArraysDemo2 {
      public static void main(String[] args) {
  //        Integer[] a = {11,2,34,213,54,3};
          Student[] s = new Student[3];
          s[0] = new Student("谢仔", 17,22.1);
          s[1] = new Student("大朗", 18,22.3);
          s[2] = new Student("朗", 19,22.4);
          Arrays.sort(s, new Comparator<Student>() {
                      @Override
                      public int compare(Student o1, Student o2) {
                          return Double.compare(o1.getNumber(),o2.getNumber());//
                      }
                  });
                  System.out.println(Arrays.toString(s));
      }
  }
  ```

* 通过Double.compare方法，将返回的浮点型转换为整形。

# 常见算法 

## 选择排序

![image-20220820111843416](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820111843416.png)

* ```java
   public static void main(String[] args) {
          int[] a = {1,23,55,6,4};
          //选择排序是定一个位置，然后若比其他位置大，则与其他位置交换
          for (int i = 0; i < a.length - 1; i++) {
              for (int j = i+1; j < a.length; j++) {
                  if (a[i] > a[j]){
                      int temp = a[i];
                      a[i] = a[j];
                      a[j] = temp;
                  }
              }
          }
          System.out.println(Arrays.toString(a));
      }
  ```

  

## 二分查找

* ![image-20220820111953218](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820111953218.png)

* ```java
  public static void main(String[] args) {
          int[] a = {2,3,52,55,5,23,7};
          Arrays.sort(a);
          System.out.println(Arrays.toString(a));
          System.out.println(binarySort(a, 5));
      }
      public static int binarySort(int[] a, int Number){
          int left = 0;
          int right = a.length - 1;
          while (left <= right){
              int middle = (left + right) / 2;
              if (a[middle] > Number){
                  right -= 1;
              }else if (a[middle] < Number){
                  left += 1;
              }else{
                  return middle;
              }
          }
          return -1;
      }
  ```

* 

# Lambda表达式

* 基本作用

  * 简化函数式接口（只有一个抽象方法)	的匿名内部类的写法

* 有什么使用前提

  * 必须是**接口**的匿名内部类，接口中只能有一个抽象方法

* lambda的好处

  * 简化代码

* ```java
  go(() -> {System.out.println("学生跑的好快"); });
  
  @FunctionalInterface
  interface Swimming{ 
      void swim();
  }
  ```

## lambda表达式的省略规则

![image-20220820112126194](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820112126194.png)

* 
* 

![image-20220820112047485](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820112047485.png)

