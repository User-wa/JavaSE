# Set系列集合

## Set系列集合概述

* ![image-20220820114754815](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820114754815.png)

## HashSet元素无序的底层原理：哈希表

* **哈希值**是JDK按照对象的**地址**，按照某种规则算出来的int类型的**数值**
* 同一对象多次调用hashCode():返回的哈希值是相同的
* 默认情况下，不同对象的哈希值是不同的

* ![image-20220820115012146](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820115012146.png)

* ![image-20220820115240444](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820115240444.png)

* ![image-20220820115320874](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820115320874.png)

* Set集合的底层原理
  * JDK8之前，哈希表：底层使用数组＋链表组成
  * JDK8之后，哈希表：底层采用数组 + 链表 + 红黑树组成

* 哈希表的详细流程
  * ![image-20220820115338876](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820115338876.png)

## HashSet元素去重复的底层原理

* ![image-20220820115407539](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820115407539.png)

* 如果希望Set集合认为2个内容相同的对象是重复的应该怎么办？
  * 重写对象的hashCode 和 equals方法。

## 实现类：LinkedHashSet

* ![image-20220820115500957](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820115500957.png)

* 底层也是基于哈希表，使用双链表记录添加顺序

## 实现类：TreeSet（底层是红黑树）

* TreeSet集合是一定要排序的，可以将元素按指定的规则进行排序

* ![image-20220820115521432](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820115521432.png)

* 默认自动排序
  * 当为数值类型时，默认为升序
  * 当为字符串时，默认按首字母的编号升序排序（ASCII）
  * 对于自定义类型如Student对象，TreeSet无法直接排序（需要制定排序规则）

* ```java
  public class TreeSetTest {
      public static void main(String[] args) {
      Set<Student> sets = new TreeSet<>(new Comparator<Student>() {
        @Override
        public int compare(Student o1, Student o2) {
        return Double.compare(o1.getLearnMoney(), o2.getLearnMoney()) >= 0.0? 1 : -1;
              }
          });
          sets.add(new Student("小黑", 18, 2000));
          sets.add(new Student("小红", 17, 3000));
          sets.add(new Student("小蓝", 18, 2000));
  
  
          System.out.println(sets);
      }
  }
  --------------------------------------------------------------
  public class Student implements Comparable<Student>{
  
   @Override
      public int compareTo(Student o) {
          return this.age - o.age >= 0? 1 : -1;
      }
  ```

* 推荐使用第二种，重写集合的Comparator比较器对象。如果两个都重写了，就实现集合的重写。（就近）

# Collection 体系的特点，使用场景总结

* 如果希望元素可以重复，又有索引，索引查询要比较快
  * 用ArrayList集合，基于数组的
* 如果希望元素可以重复，又有索引，增删首尾操作要快
  * 用LinkedList集合，基于链表的
* 如果希望增删改查都快，但是元素不重复，无序，无索引
  * 用HashSet集合，基于哈希表的
* 如果希望增删改查都快，但是元素不重复，有序，无索引
  * 用LinkedHashSet集合，基于哈希表和双链表
* 如果要对对象进行排序
  * 用TreeSet集合，基于红黑树。后续也可以用List集合实现排序

# 补充知识：可变参数

* 形式：数据类型 ... 参数名（放在形参列表中）
* 可变参数用在形参中可以接收多个数据
* 可变参数的作用
  * 可以不传输参数，可以传输一个参数或多个参数，也可以传输一个数组
  * 可变参数在方法内部本质上就是一个数组
* 注意事项
  * 一个形参中的可变参数只能有一个
  * 可变参数只能放形参列表的最后面

# 补充知识：集合工具类Collections

* ![image-20220820115732324](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820115732324.png)

* 为Collection服务，addAll（）向collection类中加入多个元素
* 打乱List集合顺序



* ![image-20220820115711967](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820115711967.png)
* 对list集合进行排序
* 通过比较器对list集合中存放的对象进行排序

## 重构快捷键(shift + F6)

# 斗地主游戏

* ```java
  public static List<Card> cardsGross = new ArrayList<>();
      static {
          String[] cardNumber = {"3","4","5","6","7","8","9","J","Q","K","A","2"};//字符串类型要注意！{“3,4”}这样只算一个字符串
  //        System.out.println(Arrays.toString(cardNumber));
          String[] colors = {"♥","♠","♦","♣"};
  //      String card = "345678910JQKA2";
  //      String colors = "♥♠♦♣";
          int index = 0;
          for (String s : cardNumber) {
              index++;
              for (String color : colors) {
                  Card c = new Card(s, color , index);
                  cardsGross.add(c);
              }
          }
          Card c1 = new Card("小王","", ++index);
          Card c2 = new Card("大王", "", ++index);
  
          Collections.addAll(cardsGross, c1, c2);
  //        System.out.println(cardsGross);
      }
      public static void main(String[] args) {
          System.out.println("没洗前的：" + cardsGross);
          Collections.shuffle(cardsGross);
          System.out.println("洗后的：" + cardsGross);
  
          List<Card> xiaoming = new ArrayList<>();
          List<Card> xiaohua = new ArrayList<>();
          List<Card> xiaohei = new ArrayList<>();
  
          for (int i = 0; i < cardsGross.size() - 3; i++) {
              if (i % 3 == 0){
                  xiaoming.add(cardsGross.get(i));
              }else if (i % 3 == 1){
                  xiaohua.add(cardsGross.get(i));
              }else if (i % 3 == 2){
                  xiaohei.add(cardsGross.get(i));
              }
          }
          List<Card> lastThreeCards = cardsGross.subList(cardsGross.size() - 3, cardsGross.size());
          System.out.println(lastThreeCards);
          for (Card lastThreeCard : lastThreeCards) {
              xiaohei.add(lastThreeCard);
          }
  //        System.out.println(xiaohua);
  //        System.out.println(xiaohei);
          cardSorts(xiaohua);
          cardSorts(xiaoming);
          cardSorts(xiaohei);
  
          System.out.println("小明的牌" + xiaoming);
          System.out.println("小花的牌：" + xiaohua);
          System.out.println("小黑的牌：" + xiaohei);
      }
  
      public static void cardSorts(List<Card> cards) {
          Collections.sort(cards, new Comparator<Card>() {
              @Override
              public int compare(Card o1, Card o2) {
                  return o1.getIndex() - o2.getIndex();
              }
          });
      }
  -----------------------------------------------------------------
      package come.halou.d4_collection_test;
  
  public class Card {
      private String number;
      private String colors;
      private int index;
  
      public Card() {
      }
  
      public Card(String number, String colors, int index) {
          this.number = number;
          this.colors = colors;
          this.index = index;
      }
  
      public String getNumber() {
          return number;
      }
  
      public void setNumber(String number) {
          this.number = number;
      }
  
      public String getColors() {
          return colors;
      }
  
      public void setColors(String colors) {
          this.colors = colors;
      }
  
      public int getIndex() {
          return index;
      }
  
      public void setIndex(int index) {
          this.index = index;
      }
  
      @Override
      public String toString() {
          return number + colors;
      }
  }
  
  ```

## 注意点

* 面向对象编程！！！，斗地主的卡片是对象，人也是对象。但是并不会创建可以放人的集合！！List<Card>  xiaoming ,集合是一个容器，每个人都有很多的牌，则容器应该放牌，而不是人！

* 集合输出的是存放在集合中的内容

* 如果一些数据长度确定，类型确定，就应该采取数组的方式存取！！！

* 泛型规定了集合存储的数据类型，添加的时候也应该添加其相应的数据类型！

* ```java
  字符串类型要注意！{“3,4”}这样只算一个字符串
  ```

* 对于重复代码，可以放在方法中定义

* List集合中的subList方法可以截取 子集合（也是List对象）

* Collections 工具类，可以根据自定义要求排序集合中的元素

* String类型的.charAt（）方法返回的是字符，而非字符串。且两个字符相加会转换为ASCII码计算就是在后面在加“”，也只会输出整数

# Map集合体系

## Map集合概述

* ![image-20220820115851249](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820115851249.png)

* 键值对集合  ，  双列集合（每个元素包括两个数据）

* Map集合非常适合做类似购物车这样的业务场景

## Map集合体系特点

* ![image-20220820120037837](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820120037837.png)

  

## Map集合常用API

* ![image-20220820120054846](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820120054846.png)
* 还有一些取元素的！

* ```java
  Map<String, Integer> map1 = new HashMap<>();
          //1,添加元素:无序，不重复，无索引
          map1.put("人性的弱点", 1);
          map1.put("九型人格", 1);
          map1.put("高情商聊天术", 100);
          map1.put("人性的弱点", 2);
          map1.put("人格", 1);
          System.out.println(map1);
  
          //2,清空集合
  //      map1.clear();
  //      System.out.println(map1);
  
          //3,判断集合是否为null，为null返回true
          System.out.println(map1.isEmpty());
  
          //4,根据键获取对应的值(public V get(Object key))
          System.out.println(map1.get("人性的弱点"));
          System.out.println(map1.get("九型人格"));
  
          //5,根据键删除整个元素
  //        map1.remove("九型人格");
  //        map1.remove("人性的弱点");
  //        System.out.println(map1);
  
          //6,判断是否包含某个键
          System.out.println(map1.containsKey("人性的弱点"));
          System.out.println(map1.containsKey("人性"));
  
          //7,判断是否包含某个值
          System.out.println(map1.containsValue(1));
          System.out.println(map1.containsValue(22));
  
          //8,获取全部键的集合(public Set<K> keySet())
          Set<String> set1 = map1.keySet();
          System.out.println(set1);
  
          //9,获取全部值得集合(Collection<V> values())
          Collection<Integer> values = map1.values();
          System.out.println(values);
  
          //10,集合的大小
          System.out.println(map1.size());
  
          //11,合并其他map集合
          Map<String, Integer> map2 = new HashMap<>();
          Map<String, Integer> map3 = new HashMap<>();
          map2.put("halou", 1);
          map2.put("halou1", 1);
          map3.put("halou2", 2);
          map3.put("halou1", 2);
          map2.putAll(map3);
          System.out.println(map3);
          System.out.println(map2);
  ```

## Map集合的遍历方式一：键找值

* ![image-20220820120142356](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820120142356.png)

* ```java
  Set<String> strings = map1.keySet();
          for (String string : strings) {
              int values = map1.get(string);
              System.out.println(string + "--->" + values);
          }
  ```

## Map集合的遍历方式二：键值对

* ![image-20220820120205570](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820120205570.png)

* ```java
  Set<Map.Entry<String, Integer>> entries = map1.entrySet();
          for (Map.Entry<String, Integer> entry : entries){
              String key = entry.getKey();
              int value = entry.getValue();
              System.out.println(key + "=========>" + value);
          }
  ```

## Map集合的遍历方式三：lambda表达式

* ![image-20220820120229542](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820120229542.png)

* ```java
  map1.forEach(new BiConsumer<String, Integer>() {
              @Override
              public void accept(String s, Integer integer) {
                  System.out.println(s + "------->" + integer);
              }
          });
  ------------------------------------------------------------
  
  map1.forEach((key, value) -> System.out.println(key + "------->" + value));
  ```

## 小案例（统计哪个景点投票人数max）

* ```java
   String[] s = {"A", "B", "C", "D"};//数据类型确定，长度确定
          StringBuilder sb = new StringBuilder();
          Random ra = new Random();
          for (int i = 0; i < 80; i++) {
              sb.append(s[ra.nextInt(s.length)]);
          }
          System.out.println(sb);
    		//由于字符串的charAt方法返回的是字符，所以将类型改为字符型
          Map<Character, Integer> map = new HashMap<>();
          for (int i = 0; i < sb.length(); i++) {
              if (map.containsKey(sb.charAt(i))){
                  map.put(sb.charAt(i), map.get(sb.charAt(i)) + 1);
              }
              else {
                  map.put(sb.charAt(i), 1);
              }
          }
          System.out.println(map);
  ```

## Map集合的实现类HashMap

* ![image-20220820120333340](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820120333340.png)

* HashMap和HashSet的底层原理是一样的，都是基于哈希表。
* ![image-20220820120510579](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820120510579.png)

* 由键决定：无序，不重复，无索引
* 键的唯一性要依赖hashCode方法和equals方法，需要重写。

## Map集合的实现类LinkedHashMap

* ![image-20220820120821055](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820120821055.png)

* 由键决定（有序，不重复，无索引）,有序是指存储，和取出的顺序是一样的

## Map集合的实现类TreeMap

* ![image-20220820120749880](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820120749880.png)

* 默认根据排序元素来去除重复值

## Map集合实现类特点

![image-20220820120847029](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820120847029.png)

## 集合嵌套

* ```java
  public class Combanation {
      public static void main(String[] args) {
          Map<String, List<String>> map = new HashMap<>();
          //在map集合中嵌套了一个List集合
  
          List<String> xiaoming = new ArrayList<>();
          Collections.addAll(xiaoming, "A", "B", "C");
          map.put("小明", xiaoming);
          List<String> xiaohua = new ArrayList<>();
          Collections.addAll(xiaohua, "A", "C");
          map.put("小花", xiaohua);
          List<String> xiaohei = new ArrayList<>();
          Collections.addAll(xiaohei, "A", "B", "C", "D");
          map.put("小黑", xiaohei);
          System.out.println(map);
  
          Map<String, Integer> map1 = new HashMap<>();
          Collection<List<String>> values = map.values();
          for (List<String> value : values) {
              for (String s : value) {
                  if (map1.containsKey(s)){
                      map1.put(s, map1.get(s) + 1);
                  }
                  else {
                      map1.put(s, 1);
                  }
              }
          }
          System.out.println(map1);
      }
  }
  
  ```

