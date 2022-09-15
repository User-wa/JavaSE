# 单元测试

## 单元测试概述

* ![image-20220904191703098](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904191703098.png)

## 单元测试快速入门

* ![image-20220904192049175](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904192049175.png)

* ![image-20220904193411631](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904193411631.png)

* ```java
  //注意事项：必须是公开的，无返回值的，没有形参的非静态测试方法
  //         测试方法必须使用@Test
  public class TestUserServer {
      @Test
      public void testCheek(){
          UserServer us = new UserServer();
          String rs = us.cheek("admin", "1234");
  
          Assert.assertEquals("错误了哦", "登录成功", rs);//检验预期值和实际值是否一样
  
      }
  
      @Test
      public void testArithmetic(){
          UserServer us = new UserServer();
          us.arithmetic();
      }
  }
  ```

## 单元测试常用注解

* ![image-20220904210003603](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904210003603.png)

* ![image-20220904210018946](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904210018946.png)

# 反射

## 反射概述

* ![image-20220904210540136](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904210540136.png)

## 反射获取类对象

* ![image-20220904211438938](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904211438938.png)

* ```java
  //得到class对象,通过class类中的forname(全限名：包名 + 类名)
  Class c = Class.forName("come.halou.d2_reflect_class.Student");
  System.out.println(c);
  
  //类名.class
  Class c1 = Student.class;
  System.out.println(c1);
  
  //对象.getclass()获取对象类的Class对象
  Student s = new Student();
  Class c2 = s.getClass();
  System.out.println(c2);
  ```

## 反射获取构造器对象

* ![image-20220904214819136](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904214819136.png)

* ```java
  public class Test {
      @org.junit.Test
      public void test(){
          Class c = Student.class;
          //提取类中的全部的构造器对象
          Constructor[] constructors = c.getDeclaredConstructors();
          //.遍历构造器
          for (Constructor constructor : constructors) {
              System.out.println(constructor.getName() + "=======>" + constructor.getParameterCount());
          }
  
      }
  
      @org.junit.Test
      public void test2() throws Exception {
          Class c = Student.class;
          // 定位某个有参构造器
          Constructor declaredConstructor = c.getDeclaredConstructor(String.class, int.class);
          // 定位单个构造器对象 (按照参数定位无参数构造器)
          Constructor declaredConstructor1 = c.getDeclaredConstructor();
          // 如果遇到了私有的构造器，可以暴力反射
          declaredConstructor1.setAccessible(true);//打开私有权限
  
          Student s1 = (Student) declaredConstructor1.newInstance();
  
          Student s = (Student) declaredConstructor.newInstance("大哥大", 100);
  
          System.out.println(s);
          System.out.println(declaredConstructor.getName() + "=======>" + declaredConstructor.getParameterCount());
  
  
      }
  }
  ```

## 反射获取成员变量对象

* ![image-20220905172416930](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220905172416930.png)

* ```java
  //得到类对象
  Class c = Student.class;
  //得到成员变量
  Field[] declaredFields = c.getDeclaredFields();
  //查看成员变量
  for (Field declaredField : declaredFields) {
      System.out.println(declaredField.getName() + "=====>" + declaredField.getType());
  }
  ```

* ```java
  //得到类对象
  Class c = Student.class;
  //得到成员变量
  Field declaredField = c.getDeclaredField("name");
  //给成员变量赋值
  Student s = new Student();
  
  declaredField.setAccessible(true);//打开私有权限！
  
  declaredField.set(s, "大海");
  System.out.println(s);
  //得到成员变量的值
  String s1 = (String) declaredField.get(s);
  System.out.println(s1);
  ```

## 反射获取方法对象

* ![image-20220905175219145](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220905175219145.png)

* 代码略

## 反射的作用-绕过编译阶段为集合添加数据

* ![image-20220905175658329](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220905175658329.png)

* ```java
  List<String> stringList1 = new ArrayList<>();
  List<Integer> stringList2 = new ArrayList<>();
  
  Class c = stringList1.getClass();
  Class c2 = stringList2.getClass();
  
  System.out.println(c);
  System.out.println(c2);
  
  System.out.println(c == c2);//true
  
  stringList1.add("你好");
  //        stringList1.add(12);
  
  //反射
  Method method = c.getMethod("add", Object.class);
  method.invoke(stringList1, 12);
  method.invoke(stringList1, false);
  System.out.println(stringList1);
  //更简单
  List list = stringList1;//赋值地址
  list.add(23);
  System.out.println(stringList1);
  ```

## 反射的作用-通用框架的底层原理

* ```java
  public static void getFields(Object o){
      try (
          PrintStream ps = new PrintStream(new FileOutputStream("junit-reflect-annotation-proxy-app\\src\\test2.txt", true));//允许追加
      ){
          Class c = o.getClass();
          ps.println("===========" + c.getSimpleName() + "==================");
  
          Field[] declaredFields = c.getDeclaredFields();//得到类对象的成员变量
  
          for (Field declaredField : declaredFields) {
              String name = declaredField.getName();
  
              declaredField.setAccessible(true);
  
              String s = declaredField.get(o) + "";
              ps.println(name + "=" + s);
          }
      } catch (Exception e) {
          e.printStackTrace();
      }
  }
  ```

* ![image-20220905191604662](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220905191604662.png)

# 注解

## 注解的概述

* 注解是对JAVA中的类，方法，成员变量做标记，然后进行特殊处理。例如JUnit中的@Test方法。

## 自定义注解

* ![image-20220905193033952](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220905193033952.png)

* ```java
  //    @Book(name = "骆驼祥子", authors = "老舍")
  //    @Book(骆驼祥子")
      @Book(value = "骆驼祥子",author = "l")
  ```

* ```java
  //public @interface Book {
  //    String name();
  //    String[] authors();
  //}
  public @interface Book {
  //    String value();
      String value();//value比较特殊，如果没有其他变量或其他变量有默认值，可以直接写取值。
      String author();
  }
  ```

## 元注解

* ![image-20220905195736224](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220905195736224.png)

* ![image-20220905195753113](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220905195753113.png)

* ```java
  @Retention(RetentionPolicy.RUNTIME)
  @Target(ElementType.METHOD)
  public @interface Book {
  //    String value();
      String value();
      String author();
  }
  ```

## 注解解析

* ![image-20220905205430630](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220905205430630.png)
* ![image-20220905201543916](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220905201543916.png)

* ```java
  public class AnnotationDemo2 {
      @Test
      public void testClass() {
          Class c = BookStore.class;
  //        Annotation annotation = c.getAnnotation(Bookk.class);//多态的方式，不能获取实现类独有方法
          if (c.isAnnotationPresent(Bookk.class)) {//判断是否存在注解
              Bookk bookk = (Bookk) c.getAnnotation(Bookk.class);
              System.out.println(bookk.value());
              System.out.println(bookk.price());
              System.out.println(Arrays.toString(bookk.authors()));
          }
      }
  
      @Test
      public void testMethod() throws Exception {
          Class c1 = BookStore.class;
  //        Annotation annotation = c.getAnnotation(Bookk.class);//多态的方式，不能获取实现类独有方法
          Method m = c1.getDeclaredMethod("test");
  
          if (m.isAnnotationPresent(Bookk.class)){
              Bookk bookk = (Bookk) m.getAnnotation(Bookk.class);
              System.out.println(bookk.value());
              System.out.println(bookk.price());
              System.out.println(Arrays.toString(bookk.authors()));
          }
  
  
      }
  
  }
  @Bookk(value = "《斗罗大陆》", price = 22.5, authors = "发疯的蜗牛，ww")
  class BookStore{
      @Bookk(value = "《妖神记》", price = 23.5, authors = "蝉，ww")
      public void test(){
  
      }
  }
  ```

# 注解的应用场景一：JUnit框架 

* ![image-20220905204311875](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220905204311875.png)

* ```java
  public class AnnotationDemo3 {
      @MyTest
      public void test1(){
          System.out.println("test1执行了");
      }
  
  
      public void test2(){
          System.out.println("test2执行了");
      }
  
      @MyTest
      public void test3(){
          System.out.println("test3执行了");
      }
  
      public static void main(String[] args) throws Exception {
          AnnotationDemo3 ad3 = new AnnotationDemo3();
          Class c = AnnotationDemo3.class;
  
          Method[] m = c.getDeclaredMethods();
          for (Method method : m) {
              if (method.isAnnotationPresent(MyTest.class)){
                  //ad3.method//通过类对象得到了类中的方法，需要用到类中的方法的API！！！
                  method.invoke(ad3);
              }
          }
  
      }
  }
  ```

# 动态代理（设计模式）

## 动态代理概述，快速入门

* ```java
  public class ProxyDemo1 {
      public static void main(String[] args) {
          Star s = new Star("杨超越");
          //        s.dance();
          //        s.sing();
          Skill proxy = GetProxy.getProxy(s);//生成代理对象，代理对象也实现了接口
          proxy.dance();
          proxy.sing();
      }
  }
  ```

* ```java
  public interface Skill {
      void sing();
      void dance();
  }
  ```

* ```java
  public class Star implements Skill{
      private String name;
  
      public Star(String name) {
          this.name = name;
      }
  
      @Override
      public void sing() {
          System.out.println(name + "正在唱歌~~~");
      }
  
      @Override
      public void dance() {
          System.out.println(name + "正在跳舞~~~");
      }
  }
  ```

* ```java
  public class GetProxy {
      public static Skill getProxy(Star s){
          return (Skill) Proxy.newProxyInstance(s.getClass().getClassLoader(), s.getClass().getInterfaces(), new InvocationHandler() {
              @Override
              public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                  System.out.println("先收费");
                  Object invoke = method.invoke(s, args//参数);
                  System.out.println("送回来");
                  return invoke;
              }
          });
      }
  }
  
  ```

* ![image-20220906185505036](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220906185505036.png)

* ![image-20220906185532942](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220906185532942.png)

## 动态代理的应用案例：做性能分析，代理的好处小结

* ![image-20220906191913457](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220906191913457.png)

* ```java
   @Override
      public String cheekName(String name) {
          System.out.println("正在按照名字查找" + name);
          try {
              Thread.sleep(2000);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          System.out.println("查找完成");
          return name;
      }
  
      @Override
      public void dance() {
          System.out.println(name + "正在跳舞~~~");
          try {
              Thread.sleep(3000);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          System.out.println(name + "跳舞完成~~~");
  
      }
  
      @Override
      public void sing() {
          System.out.println("唱歌正在进行");
          try {
              Thread.sleep(4000);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
      }
  ```

* ```java
  public static Skill getProxy(Star s){
      return (Skill) Proxy.newProxyInstance(s.getClass().getClassLoader(), s.getClass().getInterfaces(), new InvocationHandler() {
          @Override
          public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
              long startTime= System.currentTimeMillis();
              Object invoke = method.invoke(s, args);//不需要在实现类中的方法中再重复写测试性能的代码！！
              long endTime= System.currentTimeMillis();
              System.out.println(method.getName() + "的性能是: " + (endTime - startTime)/ 1000 + "s");
              return invoke;
          }
      });
  }
  ```



