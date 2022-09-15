# File类的概述

* ![image-20220825184953776](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220825184953776.png)

* ![image-20220825185011259](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220825185011259.png)

* ```java
    //创建文件对象
          //File f = new File("D:\\threeKillphoto\\1c950a7b02087bf440903ef2984b442510dfcfca.jpeg");
          //通过正斜杠完成对绝对路径的装换
  //        File f = new File("D:/threeKillphoto/1c950a7b02087bf440903ef2984b442510dfcfca.jpeg");
          //提供方法
  //        File f = new File("D:" + File.pathSeparator + "threeKillphoto" + File.pathSeparator + "1c950a7b02087bf440903ef2984b442510dfcfca.jpeg");
  //        System.out.println(f.length());
  
          //相对路径(相对于本项目)和绝对路径
          File f = new File("file-io-app/src/a.txt");
          File f1 = new File("D:\\threeKillphoto\\1c950a7b02087bf440903ef2984b442510dfcfca.jpeg");
          System.out.println(f.length());
  
          //File创建对像，可以是文件也可以是文件夹,
          File file = new File("D:/threeKillphoto");
          System.out.println(file.exists());//文件夹也是一个文件对象，拿取的是文件夹，而不是文件夹中的所有文件
  
  
      }
  ```

# File类常用API

## 判断文件类型，获取文件信息

* ![image-20220825185655422](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220825185655422.png)

* ```java
   File f = new File("D:\\threeKillphoto\\小爱.jpeg");
          System.out.println(f.getAbsolutePath());
          System.out.println(f.getPath());
          System.out.println(f.isFile());//true
          System.out.println(f.isDirectory());//false
          System.out.println(f.exists());
          System.out.println(f.getName());
          long time = f.lastModified();
          System.out.println("上一次修改的时间是" + new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(time));


```java
      File f1 = new File("file-io-app\\src\\a.txt");
      System.out.println(f1.getAbsolutePath());//会显示绝对路径
      System.out.println(f1.getPath());
```


## 创建文件，删除文件功能

* 	![image-20220825201804900](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220825201804900.png)
* 	删除文件夹可以删除文件，空文件夹。默认不能删除非空文件夹。

  ```java
//几乎不用
          File file = new File("file-io-app\\src\\a.txt");
          System.out.println(file.createNewFile());
          File file1 = new File("file-io-app\\src\\ab.txt");
          System.out.println(file1.createNewFile());
          //mkdir创建一级目录
          File file2 = new File("D:\\threeKillphoto\\aaa");
          file2.mkdir();
          //mkdirs创建多级目录
          //经常用
          File file3 = new File("D:\\threeKillphoto\\aaa\\ab\\abb");
          file3.mkdirs();
          //删除
          System.out.println(file3.delete());//true
          System.out.println(file2.delete());//false
  ```

## 遍历文件夹

* ![image-20220825203632470](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220825203632470.png)

* ```java
  //操作一级文件，只返回字符串
          File f = new File("D:\\threeKillphoto");
          String[] a = f.list();
          for (String s : a) {
              System.out.println(s);
          }
          //获取当前文件夹中的所有一级对象，并返回对象数组。
          File[] files = f.listFiles();
          for (File file : files) {
              System.out.println(file.getAbsoluteFile());
          }
          //注意事项
          File file = new File("D:\\aaaaa");
          File[] files1 = file.listFiles();
          System.out.println(files1);
          
          File file1 = new File("D:\\threeKillphoto\\小爱.jpeg");
          File[] files2 = file1.listFiles();
          System.out.println(files2);
          
          File file1 = new File("D:\\threeKillphoto\\aaa");
          File[] files2 = file1.listFiles();
          System.out.println(files2);
          System.out.println(Arrays.toString(files2));
  ```

# 方法递归

## 递归的形式和特点

* ![image-20220825205620775](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220825205620775.png)

## 递归的算法流程，核心要素

* ![image-20220825210520289](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220825210520289.png)

* ![image-20220825210421041](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220825210421041.png)

*  f(n) = 1+2+3...+n; f(n-1) = 1 + 2..... + n-1; f(n) - f(n-1) = n;   f(n) =  f(n-1)  + n;

## 递归的经典问题

* 猴子偷桃

* ```java
  public static void main(String[] args) {
          //猴子偷桃
          // f(x) 第一天的桃子 - f(x)/2 - 1 = f(x + 1) 第二天的桃子
          //有时候会发现终结方向不一致，要进行转置。
          System.out.println(f(1));
      }
  
      private static int f(int i) {
          if (i == 10){
              return 1;
          }else {
              return 2 * f(i + 1) + 2;
          }
      }
  ```

## 非规律化递归案例-文件搜索

* ![image-20220826072736253](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220826072736253.png)

* ```java
   public static void main(String[] args) {
          cheekFile(new File("D:\\"), "小爱.jpeg");
      }
    
      private static void cheekFile(File dir, String fileName) {
          if (dir != null && dir.isDirectory() ){
              File[] files = dir.listFiles();
              if (files != null && files.length > 0){
                  for (File file : files) {
                      if (file.isFile()){
                          if (file.getName().contains(fileName)){
                              System.out.println("找到了" + file.getAbsolutePath());
                          }
                      }else {
                          cheekFile(file , fileName);//当它结束之后，会自动循环
                      }
                  }
              }
          }else{
              System.out.println("请输入正确的入口");
          }
      }
  ```

## 非规律化递归案例-啤酒问题

* ```java
   //啤酒问题 4个盖子换一瓶，2个空瓶可以换一瓶，啤酒两元一瓶，问10元可以换多少瓶，剩余多少空瓶和盖子。
          buy(10);
          System.out.println(allAlcoholNumber);
          System.out.println(lastBottle);
          System.out.println(lastCover);
      }
    
      private static void buy(int money) {
          int number = money / 2;
          allAlcoholNumber += number;
    
          int coverCount = 0;
          int bottleCount = 0;
          if ((lastCover + number) >= 4) {
              coverCount += (lastCover + number) / 4;
          }
          lastCover = (lastCover + number) % 4;
    
          if ((lastBottle + number) >= 2) {
              bottleCount += (lastBottle + number) / 2;
          }
          lastBottle = (lastBottle + number) % 2;
          
          int money1 = (coverCount * 2) + (bottleCount * 2);//换钱的时候,是用能买多少瓶子算！
          if (money1 >= 2) {
              buy(money1);
          }
      }
  ```

# 字符集

## 常见字符集

* 计算机可以给人类字符进行编号存储，这套编号规则就是字符集
* ![image-20220826125037188](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220826125037188.png)

## 字符集的编码，解码操作

* ![image-20220826130018988](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220826130018988.png)

* ```java
   String s = "123你好";
          byte[] bytes = s.getBytes();
          System.out.println(bytes.length);//默认字符集UTF-8
          System.out.println(Arrays.toString(bytes));
    
          byte[] bytes = s.getBytes("GBK");
          System.out.println(bytes.length);//
          System.out.println(Arrays.toString(bytes));
    
          String s1 = new String(bytes);//转换前和转换后的编码方式要一样，否则会出现乱码,默认UTF-8.
          System.out.println(s1);
    
          String s1 = new String(bytes, "GBK");//转换前和转换后的编码方式要一样，否则会出现乱码
          System.out.println(s1);
  ```

# IO流概念

* 读写数据
* ![image-20220826131413895](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220826131413895.png)
* 字节流处理音乐，视频比较好，而字符流更适合处理文本

# 字节流的使用

* ![image-20220827121528175](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827121528175.png)

## 文件字节输入流：每次读取一个字节

* ![image-20220826133204970](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220826133204970.png)

* ![image-20220826133143084](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220826133143084.png)

* ```java
  InputStream i = new FileInputStream("file-io-app\\src\\a.txt");//多态
  
          //像水滴一样，往下流。
          int b = i.read();
          System.out.println((char)b);
          int b1 = i.read();
          System.out.println((char)b1);
          int b2 = i.read();
          System.out.println((char)b2);
          int b3 = i.read();
          System.out.println((char)b3);
          int b4 = i.read();
          System.out.println((char)b4);
  ```

## 文件字节输入流：每次读取一个字节数组

* ![image-20220827075814614](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827075814614.png)

* ```java
   InputStream f = new FileInputStream("file-io-app\\src\\a.txt");
          byte[] buttle = new byte[3];
    
          int number = f.read(buttle);
          System.out.println(number);
          System.out.println(new String(buttle));
    
          int number1 = f.read(buttle);
          System.out.println(number1);
          System.out.println(new String(buttle));
    
          int number2 = f.read(buttle);
          System.out.println(number2);
          System.out.println(new String(buttle));
          //s d f
          //c d f
          int number3 = f.read(buttle);
          System.out.println(number3);
          System.out.println(new String(buttle));//只存入了一滴，会返回上一次的剩余的两滴，有中文字符可能出现乱码
    
          int number3 = f.read(buttle);
          System.out.println(number3);
          System.out.println(new String(buttle, 0 , number3));//输出桶里面从0到number3的数据，避免
           // s d f
           // c d f  的出现
          //循环倒出桶里面的水滴
          int number = 0;
          while((number = f.read(buttle)) != -1){
              System.out.println(new String(buttle, 0, number));
          }
  ```

## 文件字节输入流：一次性读完全部字节

* ![image-20220827120438963](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827120438963.png)

* ```java
  File f1 = new File("file-io-app\\src\\a.txt");
          InputStream f = new FileInputStream(f1);
  
          byte[] b = new byte[(int) f1.length()];
          int len = f.read(b);
          System.out.println(len);
          System.out.println(f1.length());
          System.out.println(new String(b));
          
          //API
          byte[] c = f.readAllBytes();
          System.out.println(f1.length());
          System.out.println(new String(c));
  ```

## 文件字节输出流：写字节数据到文件

* ![image-20220827121509420](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827121509420.png)

* ![image-20220827122949812](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827122949812.png)

* ```java
  OutputStream out = new FileOutputStream("file-io-app\\src\\abc.txt", true);//是否追加数据
  
          out.write('a');
          out.write(97);
          out.write("\r\n".getBytes(StandardCharsets.UTF_8));
  //        out.write('哈');只能写入单个字节
  
          byte[] b1 = {'a', 'b', 'c'};
          out.write(b1);
          out.write("\r\n".getBytes(StandardCharsets.UTF_8));//换行写入
  
          byte[] b = "哈喽你好呀".getBytes();
          out.write(b);
          out.write("\r\n".getBytes(StandardCharsets.UTF_8));
  
          out.write(b, 0, 6);
          out.write("\r\n".getBytes(StandardCharsets.UTF_8));
  
          out.flush();//刷新数据,清理缓冲区
          out.close();//关闭流，关闭包含刷新，关闭后流不可以继续使用了
  ```

## 文件拷贝

* ![image-20220827132512335](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827132512335.png)

* ```java
  InputStream in = null;
          OutputStream out = null;
          try {
              //创建字节输入管道
              in = new FileInputStream("D:\\a_schoolMessage\\关于数据库\\人事部门管理系统_王飞扬\\20125082044-王飞扬\\演示录像.mp4");
  
              //创建字节输出管道
              out = new FileOutputStream("D:\\a_schoolMessage\\newMovie.mp4");//一定要写具体到文件名，否则拒绝访问
  
              //定义字节数组传过去
              byte[] b = new byte[1024];
              int num;
              while ((num = in.read(b)) != -1){
                  //out.write(b);//可能会出现最后一桶水异常问题
                  out.write(b, 0, num);
              }
              System.out.println("复制完毕");
              out.close();
              in.close();
          } catch (IOException e) {
              e.printStackTrace();
          }
  ```

* 字节流适合做一切文件数据的拷贝，因为任何文件的底层都是字节，拷贝是一字不漏的转移字节，只要前后文件格式，编码一致没有任何问题就可以进行文件拷贝。

# 资源释放的方式

## try-catch-finally

* ```java
  InputStream in = null;
          OutputStream out = null;
          try {
              //创建字节输入管道
              in = new FileInputStream("D:\\a_schoolMessage\\关于数据库\\人事部门管理系统_王飞扬\\20125082044-王飞扬\\演示录像.mp4");
  
              //创建字节输出管道
              out = new FileOutputStream("D:\\a_schoolMessage\\newMovie.mp4");//一定要写具体到文件名，否则拒绝访问
  
              //定义字节数组传过去
              byte[] b = new byte[1024];
              int num;
              while ((num = in.read(b)) != -1){
                  out.write(b, 0, num);
              }
              System.out.println("复制完毕");
  
          } catch (IOException e) {
              e.printStackTrace();
          } finally {//最后不管程序运行成功与否，都会执行finally语句！
                     //哪怕上面有return语句执行，也必须先执行完这里才可以
                     //开发中不建议在这里加return，如果加了，返回的永远是这里的数据了，这样会出现问题！
              try {
                  if (out != null)out.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
              try {
                  if (in != null)in.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
  ```

## try-catch-resourse

* 自动释放资源，代码简洁。

* ![image-20220827150301865](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827150301865.png)

* 建议使用JDK7在try（）中定义资源。其中括号中的都应该是实现closeable/AutoCloseable接口。

# 字符流的使用

## 文件字符输入流：一次读取一个字符

* ![image-20220827190925917](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827190925917.png)
* 字符流的好处：读取中文字符时不会出现乱码（如果代码和文件编码一致）
* 坏处：性能较差

* ```java
   Reader r = new FileReader("file-io-app\\src\\a.txt");
    
          //返回编号
          int n = r.read();
          System.out.println((char)n);
    
          int n1 = r.read();
          System.out.println((char)n1);
          
          int num;
          while ((num = r.read()) != -1){
              System.out.print((char)num);
          }
  ```

## 文件字符输入流：一次读取一个字符数组

* ```java
   //创建一个文件字符输入流与源文件相连
          Reader r = new FileReader("file-io-app\\src\\ab.txt");
          //用循环，每次读取一个字符数组的数据
          char[] c = new char[1024];
          int len;
          while ((len = r.read(c)) != -1){//读取字符的个数
            String rs = new String(c, 0, len);
              System.out.print(rs);
  ```

* 读取的性能得到提升

## 文件字符输出流

* ![image-20220827195312267](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827195312267.png)

* ![image-20220827195321969](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827195321969.png)

* ![image-20220827195338698](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827195338698.png)

* ![image-20220827195350188](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220827195350188.png)

* ```java
   //定义一个字符输出管道
          Writer w = new FileWriter("file-io-app\\src\\a.txt");
    
          //输出一个字符
          w.write(97);
          w.write('a');
          w.write('你');
          w.write("\r\n");
    
          //输出字符串
          w.write("你好呀");
          w.write("小猪猪");
          w.write("\r\n");
    
          //输出字符数组
          char[] butter = "我不太好呢，皮特".toCharArray();
          w.write(butter);
          w.write("\r\n");
    
          //输出字符串的一部分
          w.write("小姐姐", 1, 2);
          w.write("\r\n");


          //输出字符数组的一部分
          w.write(butter, 0,4);
          w.write("\r\n");
      
          w.flush();
          w.close();
  ```

* 
