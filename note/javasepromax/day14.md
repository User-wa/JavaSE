# XML

## XML概述

* ![image-20220906194535548](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220906194535548.png)

* 可扩展的标记语言的意思是标签的名字可以任意取。

## XML的创建，语法规则

* ![image-20220906200823602](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220906200823602.png)

* ```xml
  <?xml version="1.0" encoding="utf-8" ?><!-- 文档声明 -->
  <!-- 注释：根标签有且仅能只有一个 -->
  <Student>
      <name>蓝朋友</name>
      <sex>男</sex>
      <age>18</age>
      <info>
          <hobby>打游戏</hobby>
          <dairy>敲代码</dairy>
      </info>
      <sql>
          <![CDATA[
          select * from student where age < 19;
          ]]>
          select * from student where age &lt; 19;
          select * from student where age &lt; 19 &amp;&amp; age > 13;
      </sql>
  </Student>
  ```

## XML文档约束方式一：DTD约束

* ![image-20220906202742955](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220906202742955.png)

## XML文档约束方式二：schema约束

* ![image-20220906203234838](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220906203234838.png)

# XML解析技术

## XML解析技术概述

* ![image-20220906204006940](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220906204006940.png)

* 使用的是Dom解析的常用技术框架：Dom4J

## Dmo4J解析XML文件

* 不是JAVA中的jar包，需要导入

* ![image-20220906204352447](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220906204352447.png)

* ```java
  //创建dom4j解析对象
  SAXReader sr = new SAXReader();
  
  //把XML文件加载到内存中成为document对象
  //        Document read = sr.read(new File("javasepromax\\xml-app\\src\\books.xml"));
  
  
  //通过类对象的getResourceAsStream方法，直接到/（src）下寻找文件
  InputStream is = dom4jHelloWorldDemo1.class.getResourceAsStream("/books.xml");//更好！
  Document read1 = sr.read(is);
  

  //读取根元素对象
  System.out.println(read1.getRootElement().getName());
  ```
  
* ![image-20220906213115982](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220906213115982.png)

## Dom4j解析XML文件中的各种节点

* ![image-20220907085527937](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220907085527937.png)

* ```java
  @Test
  public void test() throws Exception {
      //创建dom4j解析对象
      SAXReader sr = new SAXReader();
  
      //把XML文件加载到内存中成为document对象
      //        Document read = sr.read(new File("javasepromax\\xml-app\\src\\books.xml"));
      //通过类对象的getResourceAsStream方法，直接到/（src）下寻找文件
      InputStream is = dom4jHelloWorldDemo1.class.getResourceAsStream("/books.xml");//更好！
      Document read1 = sr.read(is);
  
      //读取根元素对象
      Element rootElement = read1.getRootElement();
      System.out.println(rootElement.getName());
  
      //获取根元素的一级子元素
      List<Element> elements = rootElement.elements();
      for (Element element : elements) {
          System.out.println(element.getName());//得到元素对象要调用方法才能得到具体值
      }
  
      //获得根元素的指定一级元素
      System.out.println("==========================");
      Element firstElement = rootElement.element("book");
      System.out.println(firstElement.getName());
  
      //得到一级子元素的属性值
      Attribute id = firstElement.attribute("id");
      System.out.println(id.getName() + "--->" + id.getValue());
      System.out.println(firstElement.attributeValue("id"));
  
      //得到一级子元素下面的特定元素的值
      Element author = firstElement.element("author");
      System.out.println(firstElement.elementText("author"));
      System.out.println(firstElement.elementTextTrim("author"));
      System.out.println(author.getText());
      System.out.println(author.getTextTrim());
  }
  ```

## Dom4j解析Demo

* ```java
  SAXReader sr = new SAXReader();
  
  Document document = sr.read(Contact.class.getResourceAsStream("/Contacts.xml"));
  
  Element rootElement = document.getRootElement();
  
  List<Contact> contactList = new ArrayList<>();
  
  List<Element> contact = rootElement.elements("contact");
  for (Element element : contact) {
      Contact c = new Contact();
      c.setId(Integer.valueOf(element.attributeValue("id")));//将字符串转换为指定类型！
      c.setVip(Boolean.valueOf(element.attributeValue("vip")));
      c.setName(element.elementTextTrim("name"));
      c.setGender(element.elementTextTrim("gender").charAt(0));
      c.setEmail(element.elementTextTrim("email"));
      contactList.add(c);
  }
  for (Contact contact1 : contactList) {
      System.out.println(contact1);
  }
  }
  ```


# XML检索技术：Xpath

* ```java
  SAXReader sax = new SAXReader();
  //依旧要将xml导入到内存中
  Document doc = sax.read(dom4jDemo3.class.getResourceAsStream("/Contacts2.xml"));
  
  //绝对路径查找
  List<Node> nodes = doc.selectNodes("/contactList/contact/name");
  for (Node node : nodes) {
      Element element = (Element) node;
      System.out.println(element.getTextTrim());
  }
  
  //相对路径查找
  Element rootElement = doc.getRootElement();
  List<Node> nodes1 = rootElement.selectNodes("./user/contact/info/name");
  for (Node node : nodes1) {
      Element element = (Element) node;
      System.out.println(element.getTextTrim());
  }
  
  //根据属性值查找
  List<Node> nodes2 = doc.selectNodes("//@id");
  for (Node node : nodes2) {
      Attribute attribute = (Attribute) node;
      System.out.println(attribute.getName() + "--->" + attribute.getValue());
  }
  List<Node> nodes3 = doc.selectNodes("//contact[@id=1]/name");
  for (Node node : nodes3) {
      Element element = (Element) node;
      System.out.println(element.getTextTrim());
  }
  //根据全文检索//
  List<Node> nodes4 = doc.selectNodes("//contact/name");
  //        List<Node> nodes4 = doc.selectNodes("//name");
  for (Node node : nodes4) {
      Element element = (Element) node;
      System.out.println(element.getTextTrim());
  }
  ```

# 设计模式：工厂模式

* ![image-20220907102242839](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220907102242839.png)

* ```java
  public class FactoryCreate {//电脑不是自己new的，而是工厂做出来的
      public static Computer creatComputer(String name){
          switch (name){
              case "huawei":
                  Computer h = new Huawei();//要用多态的写法
  //                System.out.println( "华为启动了");
                  h.setName("huawei");
                  h.setPrice(1111);
                  return h;
              case "Mac":
  //                System.out.println("Mac启动了");
                  Computer m = new Mac();
                  m.setName("mac");
                  m.setPrice(3333);
                  return m;
              default:
                  return null;
          }
      }
  }
  ```

* ```java
  public static void main(String[] args) {
  //        Computer c = new Huawei();
  //        c.setName("华为");
  //        c.run();
          Computer huawei = FactoryCreate.creatComputer("huawei");//通过工厂来产生对象！
          huawei.run();
          System.out.println(huawei.getPrice());
          Computer mac = FactoryCreate.creatComputer("Mac");
          mac.run();
          System.out.println(mac.getPrice());
      }
  ```

# 设计模式：装饰模式

* ![image-20220907181802649](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220907181802649.png)

* ```java
  public class DecorateDemo1 {
      public static void main(String[] args) {
          InputStream is = new BufferedInputStream(new FileInputStream());
          System.out.println(is.read());
          System.out.println(is.read("halou".getBytes(StandardCharsets.UTF_8)));
      }
  }
  ```

* ```java
  public abstract class InputStream {
      public abstract int read();
      public abstract int read(byte[] bytes);
  }
  
  ```

* ```java
  public class FileInputStream extends InputStream{
      @Override
      public int read() {
          System.out.println("读了字节a");
          return 97;
      }
  
      @Override
      public int read(byte[] bytes) {
          System.out.println("读了" + Arrays.toString(bytes));
          return bytes.length;
      }
  }
  
  ```

* ```java
  public class BufferedInputStream extends InputStream{
      private FileInputStream is;
      public BufferedInputStream(FileInputStream is){
          this.is = is;
      }
      @Override
      public int read() {
          System.out.println("提供了8kb的字节池");
          return is.read();
      }
  
      @Override
      public int read(byte[] bytes) {
          System.out.println("提供了8kb的字节池");
          return is.read(bytes);
      }
  }
  
  ```



