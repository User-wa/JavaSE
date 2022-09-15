# 数组（存储一批相同数据类型的数据）

## 数组的定义

### 静态初始化数组

* ![image-20220820074742311](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820074742311.png)

* 数组是属于什么类型，数组变量名中存储的是什么？
  * 引用数据类型，存储的数组所对应的内存中的地址信息

#### 数组的访问

* **数组名称[索引]**
* 访问长度：**数组名称.length**
* 数组最大索引：**数组名.length-1**

#### 数组的注意事项

* 数组类型[] 数组名 = 数组类型 数组名[]
* 需要存放相同类型的数据
* 数组一旦定义，程序执行过程中长度，类型就固定了

### 动态初始化数组

* ![image-20220820074909973](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820074909973.png)
* ![image-20220820074945258](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820074945258.png)

#### 动态初始化数组的元素默认值

* 整型 0
* 浮点型 0.0
* 字符型 字符0
* 布尔型 false
* 引用类型 null

## 数组的遍历（数据一个一个访问）

* ![image-20220820075056627](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820075056627.png)

* 快捷键：

  ```java
  数组名.fori  
  ```

### 新知识

```java
OUT:
        while (true) {                                        
            System.out.print("请输入你要猜的整数:");
            guess = scanner.nextInt();
            for (int j = 0; j < rs.length; j++) {
                if (guess == rs[j]) {
                    System.out.println("OK");
                    System.out.println("第一次出现的位置为：" + j);
                    break OUT;
                }
```

* 通过break OUT跳出多重循环。OUT后是想要跳出的循环
* 注意没有说明次数的用while循环
* 注意逻辑性，如果没有break就会顺序执行

### 冒泡排序

* 冒泡排序的思想
  * 从头开始两两比较，把较大的元素与较小的元素进行交换
  * **每轮把当前最大的一个元素存放在数组的末尾**
* 其实现步骤
  * 定义一个外部循环判断需要几轮
  * 定义一个内部循环，控制每轮往后比较几个位置（比较的次数）
  * 如果当前值大于后一个位置的元素值，则两者交换

## 数组的内存图

### java内存分配，数组内存图

* ![image-20220820075603071](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820075603071.png)

* ![image-20220820075239779](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820075239779.png)

### 两个变量指向同一个数组

* ![image-20220820075528719](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820075528719.png)

## 数组使用的常见问题

* ArrayIndexOutofBoundsException（数组越界异常）
* NullPointerException（空指针异常） 发生情况：如果数组变量中没有存储数组的地址，而是null。

### Debug

