# API String，ArrayList

## String类

### 概述

* ![image-20220820082322391](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820082322391.png)

* 每次创建一个新的字符串，都会保存在堆内存中的字符串常量池中。而对String变量进行的修改其实都是指向了新的字符串对象。通过表达式连接的是到堆内存中

* ![image-20220820082233188](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820082233188.png)

### 对象创建原理

* ![image-20220820082650683](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820082650683.png)

* ![image-20220820082518640](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820082518640.png)

* ![image-20220820082544860](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820082544860.png)

* 通过双引号创建的字符串对象，在字符串常量池中存储同一个
* 通过new 构造器创建的字符串对象，在堆内存中分开存储

### 注意

* ![image-20220820082752389](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820082752389.png)

### 常用API-字符串内容比较

* ![image-20220820082844862](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820082844862.png)

* 因为引用类型 == 比较的是其地址

### 常用API-遍历，替换，截取，分隔操作

* ![image-20220820082926471](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820082926471.png)

* 切割和截取不一样哦

## Arraylist

### 集合的概述

* 集合中的类型是可以不确定的，长度也不确定，是动态的
* 集合中元素是可以重复的，元素也同时存在索引
* 数组中的类型是固定的，长度也是确定的
* ![image-20220820083114696](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820083114696.png)

### Arraylist集合，创建对象

* ![image-20220820083212543](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820083212543.png)

### ArrayList对于范式的支持

* ArrayList<E> 其实就是一个泛型类，可以在编译阶段约束集合对象只能操作某种数据类型
* int ->> integer  (E) 集合中只能存储引用类型，不支持基本数据类型
* ![image-20220820083354622](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820083354622.png)

### ArrayList常用API,遍历并删除元素

* ![image-20220820083313718](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820083313718.png)

* ```java
  for (int i = arr.size() - 1; i >= 0; i--) {
              if (arr.get(i) < 80){
                  arr.remove(i);
  //                i = 0;
  //                i--;
              }
          }
  ```

* 从集合中遍历元素从后面开始遍历删除，可以避免漏掉元素

* 输出arraylist，会直接得到里面的数据。

### 存储自定义类型的对象

* ![image-20220820083617489](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820083617489.png)

* ArrayList<movies> arr = new ArrayList<>();

