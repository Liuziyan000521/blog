---
title: 'StringBuffer,StringBuilder,StringJoiner的使用'
date: '2022/7/28 20:13'
cover: 'https://graphicbed.oss-cn-shanghai.aliyuncs.com/myBlog/165901061794.jpg'
tags:
  - Java基础
categories:
  - Java
abbrlink: 26170
---

# StringBuffer类
我们如果对字符串进行拼接操作等等，由于`String`是不可变类，所以每次拼接都会创建一个新的String对象，这样即耗时，又浪费空间。
`StringBuffer`和`StringBuilder`就可以解决这个问题。
StringBuffer是一个线程安全的可变字符串序列，它的所有操作方法都加上了`synchronized

## StringBuffer的构造方法
+ public StringBuffer() 无参构造 默认容量为`16`的缓冲区
+ public StringBuffer(int capacity) 指定缓冲区的构造方法
+ public StringBuffer(String str) 指定字符串内容，缓冲区大小为字符串长度+16

## StringBuffer获取容量和长度的方法
+ public int capacity() 返回缓冲区的大小
+ public int length() 返回字符数

```java
StringBuffer sb1 = new StringBuffer();
StringBuffer sb2 = new StringBuffer(32);
StringBuffer sb3 = new StringBuffer("1234");

// 获取容量的方法
System.out.printf("sb1容量为%d\n", sb1.capacity()); // sb1容量为16
System.out.printf("sb2容量为%d\n", sb2.capacity()); // sb2容量为32
System.out.printf("sb3容量为%d\n", sb3.capacity()); // sb3容量为20

// 获取字符长度
System.out.printf("sb1的长度为%d\n", sb1.length()); // sb1的长度为0
System.out.printf("sb2的长度为%d\n", sb2.length()); // sb2的长度为0
System.out.printf("sb3的长度为%d\n", sb3.length()); // sb3的长度为4
```
## StringBuffer的添加方法
+ public StringBuffer append(String str)
  将字符串追加在原本字符串的后面。如果传入`null`将在后面追加`null`，并返回当前字符串缓冲区本身。还可以传入基础数据类型以及对象
+ public StringBuffer insert(int offset,String str)
  在指定位置插入任意类型的数据，并返回字符串缓冲区本身。

```java
StringBuffer demo = new StringBuffer("demo");
System.out.printf("原本：%s\n", demo); // 原本：demo
System.out.printf("添加null：%s\n", demo.append((String) null)); // 添加null：demonull
System.out.printf("添加boolean：%s\n", demo.append(false));// 添加boolean：demonullfalse

StringBuffer sb = new StringBuffer("sb");
sb.insert(1, "11");
System.out.printf("insert(1,\"11\")：%s", sb); // insert(1,"11")：s11b
```

## StringBuffer删除的方法
+ public StringBuffer deleteCharAt(int index)
  删除指定位置的字符，返回本身
+ public StringBuffer delete(int start,int end)
  [start,end) 包头不包尾

```java
StringBuffer sb2 = new StringBuffer("sdfwqergad");
sb2.delete(1, 3);
sb2.deleteCharAt(1); 
System.out.printf("delete(1, 3)后：%s\n", sb2); // delete(1, 3)后：sqergad
System.out.printf("deleteCharAt(1)后：%s\n", sb2); // deleteCharAt(1)后：sqergad
```

## StringBuffer替换和反转的方法
+ 替换：public StringBuffer replace(int start,int end,String str)
  将[start,end)替换为str
+ 反转：public StringBuffer reserve() 字符串反转

```java
StringBuffer sb3 = new StringBuffer("aweasdzc");
sb3.replace(1, 3, "qqq");
System.out.printf("replace(1,3,\"qqq\")后：%s\n", sb3); // replace(1,3,"qqq")后：aqqqasdzc
sb3.reverse();
System.out.printf("reverse()后：%s\n", sb3); // reverse()后：czdsaqqqa
```

## StringBuffer截取的方法
+ public String substring(int start) 从指定位置截取到结尾
+ public String substring(int start,int end) 截取[start,end)返回
+ 此方法不会改变原来的。

```java
StringBuffer sb4 = new StringBuffer("asdqwe");
System.out.printf("sb4.substring(2)后：%s\n", sb4.substring(2)); // sb4.substring(2)后：dqwe
System.out.printf("sb4.substring(2,3)后：%s\n", sb4.substring(2, 4)); // sb4.substring(2,3)后：dq
```

# StringBuilder类
StringBuilder和StringBuffer中的方法完全一样
StringBuilder是线程不安全的可变字符串序列，执行效率要比StringBuffer高一点。


# StringJoiner类

## StringJoiner类的构造方法
+ new StringJoiner(CharSequence delimiter) 指定分割的字符
+ new StringJoiner(CharSequence delimiter, CharSequence prefix, CharSequence suffix)
  + delimiter：分割字符
  + prefix：首字符串
  + suffix：尾字符串

```java
StringJoiner sj = new StringJoiner(",", "pre", "suf");
sj.add("demo").add("demo2");
System.out.println(sj); // predemo,demo2suf
sj = new StringJoiner(",");
sj.add("demo3").add("demo4");
System.out.println(sj); // demo3,demo4
```