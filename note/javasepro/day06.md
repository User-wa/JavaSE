# 案件实例

## 坐飞机

* ![image-20220820080949336](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820080949336.png)

### 问题

* ```java
   Scanner scan1 = new Scanner(System.in);
          Scanner scan2 = new Scanner(System.in);
          Scanner scan3 = new Scanner(System.in);
    
          System.out.println("请输入机票原价：");
          double getScan1 = scan1.nextDouble();
          System.out.println("请输入月份：");
          int getScan2 = scan1.nextInt();
          System.out.println("请输入你要乘坐的仓类型(头等舱，经济舱)：");
          String getScan3 = scan1.next();
  ```

* 用一个Scanner就可以了

## 找素数

### 问题

* ```java
   boolean flag = false;
    
  for (int j = 2; j < i / 2; j++) {
                  if (i % j == 0) {
                      flag = true;
                      break;
                  }
              }
  ```

* 不知道是正常退出还是break退出循环，可以用信号量进行操作。

## 验证码

### 问题

* ```java
  case 0://大写字母：A 65 Z 65+25
      char a = (char) (random.nextInt(26) + 65);
      code1 += a;
      break;
  ```

* 运用强转换 int --> char 变成大小写字符

## 数组拷贝

* ![image-20220820081154702](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820081154702.png)

## 数字加密

* ```java
  //反转的另一种形式 [0,1,2,3,4,5]
  //               i         j
          for (int i = 0, j = number.length - 1; i < j; i++, j--) {
              int temp = number[i];
              number[i] = number[j];
              number[j] = temp;
          }
  ```

* 变量改变，而不是定值。

## 模拟双色球

### 数据不重复

数据不重复

* 两个循环用信号量来判断第一个循环是break退出还是正常退出


