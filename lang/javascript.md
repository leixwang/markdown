# HTML5

## HTML

## CSS

## JavaScript

### 网页中使用
1. 在网页上直接嵌入JavaScript
```javascript
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html;charset=utf-8">
  <script>
  var now = new Date();
  var hour = now.getHours();
  alert("Time: "+hour+);
  </script>
</head>
</html>
```


### JavaScript 语法
1. js的变量是区分大小写.
2. 每行结尾的分号可有可无, 如果语句的结束没有分号, 那么js会自动将这行代码的结尾作为语句的结尾.
3. 变量是弱类型
4. 使用大括号标签代码块
5. 注释
  1. 单选注释使用 `//`
  2. 多行注释 `/* ... */`

### 关键字
1. abstract
2. continue
3. finally
4. instanceof
5. private
6. this 
7. boolean
8. default
9. float
10. int
11. public
12. throw
13. break
14. do
15. for
16. interface
17. return
18. typeof
19. byte
20. double
21. function
22. long
23. short
24. true
25. case
26. else
27. goto
28. native
29. static
30. var
31. catch
32. extends
33. implements
34. new
35. super
36. void
37. char
38. false
39. import
40. null
41. switch
42. while
43. class
44. final
45. in
46. package
47. with
48. synchronized

### 数据类型
1. 数值型
  1. 整型
  2. 浮点型

2. 字符型
  * 字符型数据是使用单引号或双引号括起来的一个或多个字符.
3. 布尔型 
  * 0表示false,非0都表示true
4. 转义字符
  * \n 换行
  * \b 退格
  * \f 换页
  * \t tab符
  * \r 回车符
5. 空值
  * null 用于定义空的或者不存在的引用
6. 未定义值 
  * 已经声明但是没有赋值的变量. undefined是关键字, 用来代表未定义值

### 变量的定义和使用

1. 变量的命名规则
  * 严格区分大小写
2. 变量的声明
  * 全局变量的声明：只要是在函数外声明的变量都是全局变量。如果给一个尚未声明的变量赋值时，js会自动使用该变量创建一个全局变量。
  * 局部变量的声明：只要函数体内用`var`声明的变量就是局部变量。
3. 变量的作用域
  *

### 运算符的应用
1. 赋值运算符
2. 算术运算符
3. 比较运算符
  * == 等于 只根据表面值进行判断， 不涉及数据类型 （'11' == 11) = true
  * === 绝对等于 不仅判断表面值, 还要判断数据类型是否一样. （'11' == 11) = false
  * != 不等于 
  * !==
4. 逻辑运算符
5. 条件运算符
  * x ? r1 : r2
6. 字符串运算符
  
### 流程控制

#### if

#### switch

#### for

#### while

#### do ... while

### 函数的定义和调用
1. 函数的定义
  * function 函数名([参数]){ return}
  * 
2. 函数的调用



### 事件与处理程序
1. 事件
  * 


### window






## node.js

## meteor
