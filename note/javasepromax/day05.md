# 集合概述

* ![image-20220820112238952](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820112238952.png)

# Collection集合的体系特点

* ![image-20220820112436071](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820112436071.png)

* 集合的代表是？
  * 都是Collection接口
* 分为俩哪大常用的集合体系？
  * List体系，有序，可重复，有索引
  * Set体系，无序，不可重复，没有索引(其实现类可能表现不一样的特征)
* 运用泛型来管理输入的数据类型(集合支持泛型)
* 集合只能存引用数据类型，而不能存基本数据类型
* 集合都重写了toString方法！

# Collection集合常用API

* ![image-20220820112547500](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820112547500.png)

* ```java
   		c.remove("halou");
          System.out.println(c);
        
          System.out.println(c.contains("鹿晗"));
        
          System.out.println(c.isEmpty());
        
          System.out.println(c.size());
        
          Object[] s = c.toArray();
          System.out.println(Arrays.toString(s));
  ```

* 是单例集合的祖宗接口，功能子类可以继承使用

# Collection集合的遍历方法

## 迭代器

* iterator（）

* ```java
  Collection<String> co = new ArrayList<>();
  co.add("halou");
          co.add("nihao");
          co.add("shuaibi");
  
  Iterator<String> it = co.iterator();//返回对象，默认指向索引位置为0；
          while (it.hasNext()){
              System.out.println(it.next());
          }
  ```

* next()每次取值都要将内置指针往后移一位

* 迭代器如果出现越界，会出现nosuchelementexception异常。

* 不能遍历数组

## foreach/增强for循环

* 增强for既可以遍历集合，也可以遍历数组

* 格式：![image-20220820112643285](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820112643285.png)

* ```java
  for (String s : co) {
      System.out.println(s);
  }
  ```

## lambda表达式

* ```java
   co.forEach(new Consumer<String>() {
              @Override
              public void accept(String s) {
                  System.out.println(s);
              }
          });
  co.forEach(s ->  System.out.println(s));
  co.forEach(System.out::println);
  ```

* 

![image-20220820112741213](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820112741213.png)

# Collection集合存储自定义类型的对象

* ![image-20220820112840250](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820112840250.png)

* 集合中存储的是元素对象地址

# 常见数据结构

## 数据结构概述，栈，队列

* 栈：先进后出
* 队列：先进先出

## 数组

* ![image-20220820112951197](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820112951197.png)

* 数组是一种根据**索引**查询速度快，增删慢的模型

## 链表

* 链表增删首尾元素最快

* 数组根据索引查元素最快

* ![image-20220820113009525](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820113009525.png)

## 二叉树，二叉查找树

* ![image-20220820113034808](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820113034808.png)
* ![image-20220820113051086](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820113051086.png)

![image-20220820113112866](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820113112866.png)

* 二分查找

!![image-20220820113141680](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820113141680.png)

* 增删改查效率高，寻找效率也高。小的是存左节点，大的是存右节点。

## 平衡二叉树

* 二叉树存在的问题
  * 出现瘸子现象，导致查询的性能与单链表一样，查询速度变慢

* 平衡规则
  * 任意节点的左右两个子树的高度差不超过1，任意节点的左右两个子树都是一颗平衡二叉树

* ![image-20220820113215914](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820113215914.png)

* 左边高右边拉，右边高左边拉。如果右边拉不行，就往左边拉，再往右边拉
* 局部右旋。

## 红黑树

* ![image-20220820113236119](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820113236119.png)
* ![image-20220820113348461](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820113348461.png)

# List系列集合

## List集合特点，特有API

* ![image-20220820113445339](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820113445339.png)

* ArrayList，LinekdList：有序，可重复，有索引

* LIst的实现类的底层原理
  * ArrayList底层是基于数组实现的，根据查询元素快，增删相对较慢
  * LinkedList底层是基于双链表实现的，查询元素慢，增删首尾元素是非常快的

## List集合的遍历方式小结

* 迭代器
* foreach
* lambda表达式
* for循环（List中存在索引）[当需要对取出的元素进行操作时，用该循环形式]

## ArrayList集合的底层原理

* ![image-20220820113956449](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820113956449.png)

* 增加一个元素，size + 1，然后把数据插入。
* 当数组长度 = 元素个数时，会自动1.5倍扩容。

## LinkedList集合的底层原理

* ![image-20220820114129015](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820114129015.png)

* ```java
   //使用LinkedList集合 基于双向链表 实现栈和队列
          //栈
          LinkedList<String> stack = new LinkedList<>();//独有功能 addFirst().
          stack.addFirst("第一颗子弹");//相当于手枪加子弹，放在第一个位置
          stack.addFirst("第二颗子弹");
          /*stack.addFirst("第三颗子弹");
          stack.addFirst("第四颗子弹");*/
          stack.push("第三颗子弹");
          stack.push("第四颗子弹");
   ```


          System.out.println(stack);
          System.out.println(stack.getFirst());
          System.out.println(stack);
      
          stack.removeFirst();
          /*stack.removeFirst();
          stack.removeFirst();*/
          stack.pop();//和removeFirst一致。
          stack.pop();
          System.out.println(stack);
      
          //队列
          LinkedList<String> queue = new LinkedList<>();
          queue.addLast("第一个人");//排队往后面排。
          queue.addLast("第二个人");
          queue.addLast("第三个人");
          queue.addLast("第四个人");
      
          System.out.println(queue);
          queue.removeFirst();
          System.out.println(queue);
  ```

## 补充知识：集合的并发修改异常问题（遍历删除元素异常）

* Ctrl Alt + v 补全代码
* ![1660709234083](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\1660709234083.png)

* ```java
  ArrayList<String> co = new ArrayList<>();
          co.add("java");
          co.add("java");
          co.add("python");
          co.add("php");
          co.add("c");
          co.add("c++");
          co.add("c#");
          System.out.println(co);
  
          Iterator<String> iterator = co.iterator();
          while (iterator.hasNext()){
              if ("java".equals(iterator.next())){
                  iterator.remove();
  //                co.remove(iterator.next());//报错
              }
          }
          System.out.println(co);
          
          
         /* for (String s : co) {
              if ("java".equals(s)){
                  co.remove(s);
              }
          }
          System.out.println(co);//ConcurrentModificationException异常
  
  
          co.forEach( s -> {
                  if ("java".equals(s)){
                      co.remove("java");//报错，指针自动往前走
                  }
              });
          System.out.println(co);*/
          
          
          for (int i = 0; i < co.size(); i++) {
              if ("java".equals(co.get(i))){
                  co.remove("java");
                  i--;
              }
          }
          System.out.println(co);
          for (int i = co.size() - 1; i >= 0; i--) {
              if ("java".equals(co.get(i))){
                  co.remove("java");
              }
          }
          System.out.println(co);
  ```

# 泛型深入

## 泛型的概述，优势

* 泛型：可以在编译阶段约束操作的数据类型，并进行检查
* 泛型的格式：<数据类型>;泛型只能支持引用数据类型
* 集合体系的全部接口和实现类都是支持泛型的使用的
* 好处
  * 统一数据类型
  * 把运行期间的问题提前到了编译阶段，避免强制类型转换可能出现的异常（double转换为String类型）

## 自定义泛型类

* ![image-20220820114346760](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820114346760.png)

* ```java
  public class Animal<E>{
      public void swim(E e){
          System.out.println(e);
      }
  }
  ```

* 作用：编译阶段约定操作的数据的类型，类似于集合的作用

## 自定义泛型方法

* ![image-20220820114432372](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820114432372.png)

* ```java
  public static <T> void swim(T[] t) {
          if (t == null) {
              System.out.println(t);
          } else {
              StringBuilder builder = new StringBuilder();
              builder.append("[");
              for (int i = 0; i < t.length; i++) {
                  builder.append(t[i] + (i == t.length - 1 ? "" : ","));
              }
              builder.append("]");
              System.out.println(builder);
          }
      }
  ```

* 泛型方法的核心思想

  * 把出现泛型变量的地方全部替换成传输的真实数据类型

* 泛型方法的作用

  * 方法中可以使用泛型接收一切实际类型的参数，方法更具备通用性

## 自定义泛型接口

* ```java
  public interface Data<E> {
      void add(E e);
      void delete(int i);
      E queryById(E e);
  }
  
  
  public class TeacherData implements Data<Teacher>{
      @Override
      public void add(Teacher teacher) {
  
      }
  
      @Override
      public void delete(int i) {
  
      }
  
      @Override
      public Teacher queryById(Teacher teacher) {
          return null;
      }
  }
  
  ```

* ![image-20220820114501221](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820114501221.png)

* ![image-20220820114600668](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820114600668.png)

## 泛型通配符，上下限

* ```java
  public static void main(String[] args) {
          ArrayList<benchi> benchis = new ArrayList<>();
          benchis.add(new benchi());
          benchis.add(new benchi());
          benchis.add(new benchi());
          compete(benchis);
  
          ArrayList<baoma> baomas = new ArrayList<>();
          baomas.add(new baoma());
          baomas.add(new baoma());
          compete(baomas);
  
          ArrayList<Dog> dogs = new ArrayList<>();
          dogs.add(new Dog());
          dogs.add(new Dog());
          dogs.add(new Dog());
          compete(dogs);
      }
      public static void compete(ArrayList<? extends Car> benchis){
  
      }
  }
  class Car{
  
  }
  class Dog{
  
  }
  class benchi extends Car{
  
  }
  class baoma extends Car{
  
  }
  ```

* ![image-20220820114641484](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820114641484.png)

* ? 与 E 的使用。

