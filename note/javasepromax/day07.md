

# 创建不可变集合

* ![image-20220820121017149](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121017149.png)
* 定义不可变集合之后，里面的元素不允许被修改

* ```java
    List<String> list = List.of("1", "2", "3");
          list.add("halou");
          System.out.println(list);
  -------------------------------------------
      Exception in thread "main" java.lang.UnsupportedOperationException
  ```

# Stream流

## Stream流的概述

* ![image-20220820121124575](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121124575.png)

* Stream流是来简化集合和数组操作的API。

## Stream流的获取

* ![image-20220820121204293](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121204293.png)

* ```java
   Collection<String> list = new ArrayList<>();
          Collections.addAll(list, "halou", "hie", "jjd");
          Stream<String> stream = list.stream();
   
    Map<String, Integer> map = new HashMap();
         Stream<Map.Entry<String, Integer>> stream1 = map.entrySet().stream();
         Stream<String> stream2 = map.keySet().stream();
         Stream<Integer> stream3 = map.values().stream();
   
     int[] a = {1,2,3,4,5};
         IntStream stream4 = Arrays.stream(a);//数组中的方法
         Stream<int[]> a1 = Stream.of(a);//Stream类中的方法

## Stream流常用的API

* ![image-20220821225119437](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220821225119437.png)
* map方法是对数据的加工。

* 做数据的分析和处理，不能直接修改集合，数组中的元素，要修改的话，需要用到之前的集合处理方法。

* ```java
  Collection<String> list = new ArrayList<>();
          Collections.addAll(list, "张三风", "杨颖", "包子", "馒头", "包子", "杨洋", "杨闯");
          
          list.stream().filter( s -> s.startsWith("杨")).forEach(s -> System.out.println(s));
          System.out.println("=============");
          
          list.stream().filter(s -> s.startsWith("杨")).limit(2).forEach(s -> System.out.println(s));
          System.out.println("=============");
          
          list.stream().filter(s -> s.startsWith("杨")).skip(1).forEach(s -> System.out.println(s));//跳过前面的元素
          System.out.println("=============");
  
          System.out.println("=============");
          list.stream().map(s -> s + "好吃").forEach(s -> System.out.println(s));
          list.stream().map(s -> s + "好吃").forEach(System.out::println);//方法引用
          System.out.println("=============");
  
          list.stream().distinct().filter(s -> s.startsWith("包")).forEach(s -> System.out.println(s));
          System.out.println(list.stream().filter(s -> s.startsWith("包")).count());
          System.out.println("=============");
  
          String[] strings = {"1", "2", "3", "4"};
          String[] string2 = {"1", "2", "3", "4"};
  
          Stream<String> stream1= Arrays.stream(string2);
          Stream<String> stream2 = Stream.of(strings);
  
  //        Stream.concat(stream1, stream2).forEach(s -> System.out.println(s));
          Stream<String> concat = Stream.concat(stream1, stream2);//同一类型的流，如果不同类型，泛型中就要加入他们的父类。

## Stream流的综合应用

* ```java
  public static double sum;
      public static double allSum;
  
      public static void main(String[] args) {
          List<Employee> one = new ArrayList<>();
          one.add(new Employee("猪八戒",'男',30000 , 25000, null));
          one.add(new Employee("孙悟空",'男',25000 , 1000, "顶撞上司"));
          one.add(new Employee("沙僧",'男',20000 , 20000, null));
          one.add(new Employee("小白龙",'男',20000 , 25000, null));
  
          List<Employee> two = new ArrayList<>();
          two.add(new Employee("武松",'男',15000 , 9000, null));
          two.add(new Employee("李逵",'男',20000 , 10000, null));
          two.add(new Employee("西门庆",'男',50000 , 100000, "被打"));
          two.add(new Employee("潘金莲",'女',3500 , 1000, "被打"));
          two.add(new Employee("武大郎",'女',20000 , 0, "下毒"));
  
          // 1、开发一部的最高工资的员工。（API）
          Topperformer t1 = one.stream().max((o1, o2) -> Double.compare(o1.getSalary() + o1.getBonus(), o2.getSalary() + o2.getBonus())).map(s -> new Topperformer(s.getName(), s.getSalary() + s.getBonus())).get();
          Topperformer t2 = two.stream().max((o1, o2) -> Double.compare(o1.getSalary() + o1.getBonus(), o2.getSalary() + o2.getBonus())).map(s -> new Topperformer(s.getName(), s.getSalary() + s.getBonus())).get();
          System.out.println(t1);
          System.out.println(t2);
          // 2、统计平均工资，去掉最高工资和最低工资\
  //        double sum = 0.0;
          /*one.stream().sorted((o1, o2) -> Double.compare(o1.getSalary() + o1.getBonus(), o2.getSalary() + o2.getBonus())).skip(1).limit(one.size() - 2).map( e -> {
              if (e == null) {
                  return sum / (one.size() - 2);
              }else {
                  sum += e.getSalary() + e.getBonus();
              }
          });*/
          one.stream().sorted((o1, o2) -> Double.compare(o1.getSalary() + o1.getBonus(), o2.getSalary() + o2.getBonus())).skip(1).limit(one.size() - 2).forEach( s -> {
              sum += s.getSalary() + s.getBonus();
          });
          System.out.println("平均值为：" + sum / (one.size() - 2));
          // 3、合并2个集合流，再统计
          Stream<Employee> stream1 = one.stream();
          Stream<Employee> stream2 = two.stream();
          Stream<Employee> stream3 = Stream.concat(stream1, stream2);
          stream3.sorted((o1, o2) -> Double.compare(o1.getSalary() + o1.getBonus(), o2.getSalary() + o2.getBonus())).skip(1).limit(one.size() + two.size() - 2).forEach( s -> {
              allSum += s.getSalary() + s.getBonus();
          });
  
          // BigDecimal
          BigDecimal b = BigDecimal.valueOf(allSum);
          BigDecimal b1 = BigDecimal.valueOf(one.size() + two.size() - 2);
  
          System.out.println("平均值为：" +  b.divide(b1, 2, RoundingMode.HALF_UP ));
      }
  ```

* 用到了Stream流的max方法，重写匿名内部类放置比较方法。

* 必须用get方法才能返回集合中具体的值

*  BigDecimal需要将两个对象都要变成 BigDecimal对象才能运算。

## 收集Stream流

* ![image-20220820121238161](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121238161.png)

* ```java
  public static void main(String[] args) {
          List<String> list = new ArrayList<>();
          list.add("张无忌");
          list.add("周芷若");
          list.add("赵敏");
          list.add("张强");
          list.add("张三丰");
          list.add("张三丰");
  
          Stream<String> zhangStream = list.stream().filter(e -> e.startsWith("张"));
          List<String> zhangList = zhangStream.collect(Collectors.toList());
          System.out.println(zhangList);
          //Stream流像水流一样，是一次性的，流完就没了
          Stream<String> zhangStream1 = list.stream().filter(e -> e.startsWith("张"));
          Set<String> zhangSet1 = zhangStream1.collect(Collectors.toSet());
          System.out.println(zhangSet1);
  
          Stream<String> zhangLIst1 = list.stream().filter(e -> e.startsWith("张"));
          Object[] o = zhangLIst1.toArray();//需要Object数组接收，因为不确定Stream流中是否有别的元素。
  //        String[] s11 = zhangList.toArray(new IntFunction<String[]>() {
  //            @Override
  //            public String[] apply(int value) {
  //                return new String[value];
  //            }
  //        });
          System.out.println(Arrays.toString(o));
          System.out.println("==============================================");
  ```

* collect方法。恢复到集合或数组中.

# 异常处理

## 异常的概述和体系

* ![image-20220820121341528](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121341528.png)

* ![image-20220820121400371](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121400371.png)

* 编译时异常：（语法错误不算在异常体系中）
* 运行时异常

## 常见运行时异常

* ```java
  System.out.println("程序开始。。。。。。");
          /** 1.数组索引越界异常: ArrayIndexOutOfBoundsException。*/
          int[] arr = {1, 2, 3};
          System.out.println(arr[2]);
          // System.out.println(arr[3]); // 运行出错，程序终止
  
          /** 2.空指针异常 : NullPointerException。直接输出没有问题。但是调用空指针的变量的功能就会报错！！ */
          String name = null;
          System.out.println(name); // null
          // System.out.println(name.length()); // 运行出错，程序终止
  
          /** 3.类型转换异常：ClassCastException。 */
          Object o = 23;
          // String s = (String) o;  // 运行出错，程序终止
  
          /** 5.数学操作异常：ArithmeticException。 */
          //int c = 10 / 0;
  
          /** 6.数字转换异常： NumberFormatException。 */
          //String number = "23";
          String number = "23aabbc";
          Integer it = Integer.valueOf(number); // 运行出错，程序终止
          System.out.println(it + 1);
  
          System.out.println("程序结束。。。。。");
  ```

* 运行时异常： ArrayIndexOutOfBoundsException，NullPointerException，

ClassCastException，ArithmeticException， NumberFormatException

* 一般是程序员业务没有考虑好或者编程逻辑不严谨引起的程序错误。

## 常见编译型异常

* ![image-20220820121448731](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121448731.png)

* ```java
  public static void main(String[] args) throws ParseException {
          String date = "2015-01-12 10:23:21";
          // 创建一个简单日期格式化类：
          SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM-dd HH:mm:ss");
          // 解析字符串时间成为日期对象
          Date d = sdf.parse(date);//会出现编译型异常，不确定你是否填写正确。
          //
          System.out.println(d);
      }
  ```

## 异常的默认处理流程

* ![image-20220820121520079](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121520079.png)

* 默认处理机制并不好，一旦真的出现异常，程序立即死亡

## 编译时异常的处理机制

### throws

* throws直接抛出异常，并不处理。当抛出多个异常时，用Exception取代

* ![image-20220820121627138](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121627138.png)

* ```java
  public static void main(String[] args) throws Exception {
          System.out.println("程序开始。。。。。");
          parseTime("2011-11-11 11:11:11");
          System.out.println("程序结束。。。。。");
      }
  
      public static void parseTime(String date) throws Exception {
          SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
          Date d = sdf.parse(date);
          System.out.println(d);
  
          InputStream is = new FileInputStream("E:/meinv.jpg");
  ```

### try ... catch...

* ![image-20220820121658098](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121658098.png)

* 发生异常自己独立处理，程序可以往下执行。

### 前两者结合

* 通过向上传递Exception异常，让调用者try...catch.调用者再捕获处理

* ```java
  System.out.println("程序开始。。。。");
          try {
              parseTime("2011-11-11 11:11:11");
              System.out.println("功能操作成功~~~");
          } catch (Exception e) {
              e.printStackTrace();
              System.out.println("功能操作失败~~~");//调用者~！
          }
          System.out.println("程序结束。。。。");
      }
  												//向调用者抛出异常
      public static void parseTime(String date) throws Exception {
          SimpleDateFormat sdf = new SimpleDateFormat("yyyy、MM-dd HH:mm:ss");
          Date d = sdf.parse(date);
          System.out.println(d);
  
          InputStream is = new FileInputStream("D:/meinv.jpg");
      }
  ```

  

* ![image-20220820121725118](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820121725118.png)

## 运行时异常处理机制

* 建议在最外层调用处集中捕获处理。运行时异常是自动往上抛。

* ```java
  System.out.println("程序开始。。。。。。。。。。");
          try {
              chu(10, 0);
          } catch (Exception e) {
              e.printStackTrace();
          }
          System.out.println("程序结束。。。。。。。。。。");
      }
  
      public static void chu(int a , int b) { // throws RuntimeException{
          System.out.println(a);
          System.out.println(b);
          int c = a / b;
          System.out.println(c);
      }
  ```

## 异常处理使代码更稳健的案例

* ```java
  Scanner sc  = new Scanner(System.in);
          while (true) {
              try {
                  System.out.println("请您输入合法的价格：");
                  String priceStr = sc.nextLine();
                  // 转换成double类型的价格
                  double price = Double.valueOf(priceStr);
  
                  // 判断价格是否大于 0
                  if(price > 0) {
                      System.out.println("定价：" + price);
                      break;
                  }else {
                      System.out.println("价格必须是正数~~~");
                  }
              } catch (Exception e) {
                  System.out.println("用户输入的数据有毛病，请您输入合法的数值，建议为正数~~");
              }
          }
  ```

## 自定义异常

* ```java
  public class AgeIllegalException extends RuntimeException{
      public AgeIllegalException() {
      }
  
      public AgeIllegalException(String message) {
          super(message);
      }
  }
  ==========================================================
    public static void main(String[] args) {
          try {
              check(-23);
          } catch (AgeIllegalException e) {
              e.printStackTrace();
          }
      }
      public static void  check(int a){
          if (a < 0 || a > 200){
              throw new AgeIllegalException("你的年龄输入有误");
          }else {
              System.out.println("可以输入");
          }
      }   
  ```

* ```java
  自定义异常:
          自定义编译时异常.
              a.定义一个异常类继承Exception.
              b.重写构造器。
              c.在出现异常的地方用throw new 自定义对象抛出!
              编译时异常是编译阶段就报错，提醒更加强烈，一定需要处理！！
  
          自定义运行时异常.
              a.定义一个异常类继承RuntimeException.
              b.重写构造器。
              c.在出现异常的地方用throw new 自定义对象抛出!
              提醒不强烈，编译阶段不报错！！运行时才可能出现！！
  ```

* 注意：throw ：在方法内部直接创建一个异常对象，并从此点抛出， throws : 用在方法申明上的，抛出方法内部的异常。



