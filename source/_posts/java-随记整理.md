---
title: java_随记整理
date: 2018-04-17 17:03:18
tags: [java]
categories: [java整理总结]
---
## 用命令行运行java程序
- 找到`.java`文件所在位置
- 在`cmd`上运行`javac xx.java`，生成`.class`编译文件
- 执行`java 主类名字`(注意：此处的主类名字为`main`所在的类名，区分大小写)
- java的`String[] args`中不包括`java 主类名字`两个字符串，故`args[0]`为主类名字后面跟着的字符串
- 如果有多个`package`，则在`.../bin`路径下运行`java 包名.class名称`
- windows的目录上一级用`../`表示

## 正则表达式Pattern和Matcher类
- Pattern类：编译正则表达式后创建一个匹配模式
  - `Pattern compile(String regex)`
  由于Pattern的构造函数是私有的，不可以直接创建，所有通过静态方法`compile(String regex)`方法来创建，将给定的正则表达式编译并赋予给pattern类。
  - `String pattern()`：返回正则表达式的字符串形式，其实就是返回`regex`参数
  - `Pattern.matcher(CharSequence input)`对指定输入的字符串创建一个Matcher对象
  - `String split(CharSequence input)`分割字符串
  - `matches()`方法编译给定的正则表达式并且对输入的字符串以该正则表达式为模开展匹配，该方法适合于该正则表达式只会使用一次的情况，也就是只进行一次匹配工作，因为这种情况下并不需要生成一个matcher实例
```java
String regex = "\\?|\\*";
Pattern pattern = Pattern.compile(regex);
String patternStr = pattern.pattern();//返回\?|\*  (\为转义字符)
//////////////////////////////////////////////////////
Pattern pattern = Pattern.compile("\\?{2}");
Matcher matcher = pattern.matcher("??");
boolean matches = matcher.matches();// true
///////////////////////////////////////////////////////
String regex = "\\?|\\*";
Pattern pattern = Pattern.compile(regex);
String[] splitStrs = pattern.split("123?123*456*456");//123 123 456 456
String[] splitStrs2 = pattern.split("123?123*456*456", 2);// 123 123*456*456
//////////////////////////////////////////////////////////
String regex = "\\?|\\*";
Pattern pattern = Pattern.compile(regex);
boolean matches = pattern.matches(regex, "?");//返回true
```
- Matcher类：使用Pattern实例提供的模式信息对正则表达式进行匹配
  - `boolean matches()`尝试对整个目标字符展开匹配检测，也就是只有整个目标字符串完全匹配时才返回真值
  - `boolean lookingAt()`对前面的字符串进行匹配，只有匹配到的字符串在最前面才会返回true
  - `boolean find()`对字符串进行匹配，匹配到的字符串可以在任意位置
  - `int start()`返回当前匹配到的字符串在原目标字符串中的位置
  - `int end()`返回当前匹配的字符串的最后一个字符在原目标字符串中的索引位置
  - `String group`返回匹配到的子字符串
```java
Pattern pattern = Pattern.compile("\\?{2}");
Matcher matcher = pattern.matcher("??");
boolean matches = matcher.matches();//true
System.out.println(matches);
matcher=pattern.matcher("?");
matches = matcher.matches();//false
System.out.println(matches);
/////////////////////////////////////////////
Pattern p = Pattern.compile("\\d+");
Matcher m = p.matcher("22bb23");
boolean match = m.lookingAt();//true
System.out.println(match);
m = p.matcher("bb2233");
match= m.lookingAt();
System.out.println(match);//false
////////////////////////////////////////////////
Pattern p = Pattern.compile("\\d+");
Matcher m = p.matcher("22bb23");
m.find();// 返回true
Matcher m2 = p.matcher("aa2223");
m2.find();// 返回true
Matcher m3 = p.matcher("aa2223bb");
m3.find();// 返回true
Matcher m4 = p.matcher("aabb");
m4.find();// 返回false
//////////////////////////////////////////////////
Pattern p = Pattern.compile("\\d+");
Matcher m = p.matcher("aa22bb23");
m.find();
int start = m.start();//2
String group = m.group();//22
int end = m.end();//4
System.out.println(start);
System.out.println(group);
System.out.println(end);
```
## `getPath、getAbsolutePath、getCanonicalPath`的区别
```java
  System.out.println(System.getProperty("user.dir"));
  try {
   System.out.println("-----默认相对路径：取得路径不同------");
   File file1 =new File("..\\src\\test1.txt");
   System.out.println(file1.getPath());
   System.out.println(file1.getAbsolutePath());
   System.out.println(file1.getCanonicalPath());
   System.out.println("-----默认相对路径：取得路径不同------");
   File file =new File(".\\test1.txt");
   System.out.println(file.getPath());
   System.out.println(file.getAbsolutePath());
   System.out.println(file.getCanonicalPath());

   System.out.println("-----默认绝对路径:取得路径相同------");
   File file2 =new File("D:\\workspace\\test\\test1.txt");
   System.out.println(file2.getPath());
   System.out.println(file2.getAbsolutePath());
   System.out.println(file2.getCanonicalPath());
  } catch (IOException e) {
   e.printStackTrace();
  }
```
返回的结果如下：
```java
F:\eclipseworkspace\testejb
-----默认相对路径：取得路径不同------
..\src\test1.txt
F:\eclipseworkspace\testejb\..\src\test1.txt
F:\eclipseworkspace\src\test1.txt
-----默认相对路径：取得路径不同------
.\test1.txt
F:\eclipseworkspace\testejb\.\test1.txt
F:\eclipseworkspace\testejb\test1.txt
-----默认绝对路径:取得路径相同------
D:\workspace\test\test1.txt
D:\workspace\test\test1.txt
D:\workspace\test\test1.txt
```
- 当输入为绝对路径时，返回的都是绝对路径
- 当输入为相对路径时
  - `getPath`返回的是File构造方法里的路径，是什么就是什么，不可解析
  - `getAbsolutePath`返回的其实是`user.dir+getPath`的内容，不可解析
  - `getCanonicalPath`返回的就是标准的将符号完全解析的路径
