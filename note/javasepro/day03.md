# 分支结构（if，switch，switch穿透）

## If分支

* ![image-20220819232112259](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220819232112259.png)
  * 适合做区间匹配

## Switch分支

* switch分支
  * 适合做**值匹配**的分支选择，结构清晰。
  * 先执行表达式，再根据表达式的值对case之后的值进行匹配
  * 遇到break跳出switch分支
  * 如果都不满足，就执行default中的代码
* ![image-20220819232256672](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220819232256672.png)

## switch分支注意事项

* 表达式类型只能是byte，short，int，char，JDK5支持枚举，JDK7支持String，**不支持double，float，long**
* case给出的值不能相同，且只是允许字面量，不能是变量
* 不能忘记写break，否则出现穿透现象

## switch的穿透性

* 当case中没有break便会向下穿透。且当判断完成后将不再判断往下穿透的case值，直到有break为止。
* 能解决什么问题？
  * 存在多个case分支的功能代码是一样的，可以用穿透性把流程集中在一处处理，这样可以简化代码

# 循环结构

## For循环

* ![image-20220820073648909](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820073648909.png)
* for循环中，控制循环的变量只能在循环中使用。while循环中，控制循环的变量在循环后还可以继续使用

## while循环

* ![image-20220820073847826](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820073847826.png)
* 什么时候用for，什么时候用while？
  * 使用规范：知道循环几次，使用for。不知道循环几次，使用while。

## do-while循环

* ![image-20220820074028342](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820074028342.png)

## 死循环

* ![image-20220820074124675](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820074124675.png)

## 循环嵌套

* ![image-20220820074241917](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820074241917.png)

# 跳转关键字（break和continue）

* 注意事项
  * break：只能用于结束所在循环，或者结束所在switch分支的执行
  * continue：只能在循环中使用

# 随机数Random类

## random的使用

* ![image-20220820074511070](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820074511070.png)

![image-20220820074530357](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220820074530357.png)

* ctrl + alt + T 将代码放入指定框架中。