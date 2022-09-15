# 缓冲流

## 缓冲流概述

* ![image-20220827201316756](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827201316756.png)

* ![image-20220827201415541](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827201415541.png)

* 缓冲区在内存中，只需要在内存中读数据和写数据，速度很快
* ![image-20220827201534513](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827201534513.png)

## 字节缓冲流

* ![image-20220827203747837](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827203747837.png)

* ```java
  try ( //创建字节输入管道
                InputStream in = new FileInputStream("D:\\threeKillphoto\\小爱.jpeg");
                //创建缓冲字节输入管道（包装字节输入管道）
                InputStream bis = new BufferedInputStream(in);
                //创建字节输出管道
                OutputStream out = new FileOutputStream("D:\\a_schoolMessage\\小可爱.jpeg");//一定要写具体到文件名，否则拒绝访问
                //创建缓冲字节输出管道（包装字节输出管道）
                OutputStream bos = new BufferedOutputStream(out);
          ) {
  
              //定义字节数组传过去
              byte[] b = new byte[1024];
              int num;
              while ((num = bis.read(b)) != -1) {
                  bos.write(b, 0, num);
              }
              System.out.println("复制完毕");
          } catch (IOException e) {
              e.printStackTrace();
          }
  ```

* ![image-20220827203855045](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827203855045.png)

## 字节缓冲流的性能分析

* ![image-20220827213219275](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827213219275.png)

## 字符缓冲流

* ```java
   //缓冲字符输入流
          try (
                  Reader reader = new FileReader("file-io-app\\src\\a.txt");
                  BufferedReader br = new BufferedReader(reader);
                  Writer w = new FileWriter("io-app2\\src\\a.txt", true);
                  BufferedWriter bw = new BufferedWriter(w);
                  ){
    
              String len;
              while ((len = br.readLine()) != null){//字符缓冲输入流新特性，每次读取一行，如果有两个空行（下面没内容了），则显示null
                  bw.write(len);
                  bw.newLine();//缓冲字符输出流新特性，换行
              }
    
          }catch (Exception e){
              e.printStackTrace();
          }
  ```

* ![image-20220827220414940](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827220414940.png)

* Reader r 多态写法，可以传入实现类

### 出师表

* ```java
  try (//定义缓冲字符输入流管道，建立与源文件接口
               BufferedReader r = new BufferedReader(new FileReader("io-app2\\src\\csb.txt"));//一步到位
  
               //创建缓冲字符输出流
               BufferedWriter w = new BufferedWriter(new FileWriter("io-app2\\src\\new.txt"));
               ){
              //自己定义规则
              List<String> sizes = new ArrayList<>();
              Collections.addAll(sizes, "一", "二", "三", "四", "五", "陆", "柒", "八", "九", "十", "十一");//自定义比较规则
  
              //用List集合接收数据进行处理
              List<String> list = new ArrayList<>();
              String line;
              while ((line = r.readLine()) != null){
                  list.add(line);
              }
              Collections.sort(list, new Comparator<String>() {
                  @Override
                  public int compare(String o1, String o2) {
                      return sizes.indexOf(o1.substring(0, o1.indexOf("."))) - sizes.indexOf(o2.substring(0, o2.indexOf(".")));//取得索引，根据索引值比较大小
                  }
              });
  
              for (String s : list) {
                  w.write(s);
                  w.newLine();
              }
  
          } catch (Exception e) {
              e.printStackTrace();
          }
  ```

# 转换流

## 问题引出：不同编码读取乱码问题

* 

![image-20220828072352946](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220828072352946.png)

## 字符输入转换流

* ![image-20220828072609812](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220828072609812.png)

* ```java
   try (
  //                Reader reader = new FileReader("D:\\a_schoolMessage\\1.txt");
                  InputStream is = new FileInputStream("D:\\a_schoolMessage\\1.txt");
  //                Reader reader1 = new InputStreamReader(is);//默认还是以UTF-8的形式将字节转换成字符流，依旧会乱码，跟直接使用FileReader一样
                  Reader reader1 = new InputStreamReader(is, "GBK");//以指定的GBK编码将字节转换为字符，完美的解决问题
                  BufferedReader br = new BufferedReader(reader1);
                  ){
  
              String line;
              while ((line = br.readLine()) != null){
                  System.out.println(line);
              }
  
          } catch (Exception e) {
              e.printStackTrace();
          }
  ```

* ![image-20220828080148013](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220828080148013.png)

* 先提取源文件的原始字节流，然后根据具体编码转换成字符

## 字符输出转换流·

* ```java
   //定义一个文件字节输出流
          OutputStream os = new FileOutputStream("io-app2/src/new2.txt");
    
          //定义一个字符输出转换流,将原始的字节输出流转换为字符输出流
          Writer w = new OutputStreamWriter(os, "GBK");//指定GBK的编码方式写出去
    
          //定义缓冲字符输出流
          BufferedWriter bw =new BufferedWriter(w);
    
          bw.write("你好呀");
          bw.newLine();
          bw.write("不好吗");
    
          bw.close();//最外层的管道释放后，里面的管道也释放了
  ```

* ![image-20220828145613926](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220828145613926.png)

# 序列化对象

## 对象序列化

* 以内存为基准，将内存中的对象存储到磁盘文件中去，称为对象序列化

* ![image-20220828181129989](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220828181129989.png)

* ```java
  //定义一个学生类
          Student s = new Student("小明", "local", "1234", 19);
  
          //定义对象管道
          ObjectOutputStream oss = new ObjectOutputStream(new FileOutputStream("io-app2/src/object.txt"));
  
          //向文件中写入对象数据
          oss.writeObject(s);
  
          oss.close();
          System.out.println("写入完毕");
  
  =================================================================================
      public class Student implements Serializable {//要实现Serializable接口
      private String name;
      private String loginName;
      private String password;
      private int age;
  ```

* ![image-20220828183012241](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220828183012241.png)

## 对象反序列化

* 以内存为基准，将磁盘文件中的对象数据以字节的形式流入内存中，将数据恢复成内存中的对象

* ```java
  //创建对象字节输入管道
  ObjectInputStream ois = new ObjectInputStream(new FileInputStream("io-app2/src/object.txt"));
  
  //读取对象
  Student s = (Student) ois.readObject();
  
  //输出看看
  System.out.println(s);
  ```

* 注意事项

  ```java
  类一定要实现 implements Serializable 接口
  ```

  ```java
   private transient String password; //通过transient修饰的变量不参加序列化了
  ```

  ```java
   public static final long  serialVersionUID = 1; //在类中声明serialVersionUID当前序列化的版本号码，序列号的版本号和反序列化的版本号必须一致才不会出错
  ```



* ![image-20220828190051589](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220828190051589.png)
