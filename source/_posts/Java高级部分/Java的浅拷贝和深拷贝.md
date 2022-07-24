---
title: Java的浅拷贝(浅复制)和深拷贝(深复制)
data: '2022/7/23 23:45'
description: 网上查资料看浅拷贝和深拷贝的浅理解
cover: 'https://graphicbed.oss-cn-shanghai.aliyuncs.com/myBlog/wallhaven-mdlyoy.png'
tags:
 - java高级
 - oop
 - 拷贝
 - Java面试
categories:
 - Java
 - Java面试
abbrlink: 15940
---



# 什么是Java的浅拷贝？

​		[浅拷贝](https://baike.baidu.com/item/%E6%B5%85%E6%8B%B7%E8%B4%9D/8648181?fr=aladdin)又称浅复制，浅克隆。浅拷贝是指拷贝时只拷贝对象本身（包括对象中的基本变量），而不拷贝对象包含的引用所指向的对象，拷贝出来的对象的所有变量的值都含有与原来对象相同的值，而所有对其他对象的引用都指向原来的对象，简单地说，浅拷贝只拷贝对象不拷贝引用。



# 什么是Java的深拷贝？

​		[深拷贝](https://baike.baidu.com/item/%E6%B7%B1%E6%8B%B7%E8%B4%9D/22785317?fr=aladdin)又称为深复制，深克隆。深拷贝不仅拷贝对象本身，而且还拷贝对象包含的引用所指向的对象，拷贝出来的对象的所有变量（不包含那些引用其他对象的变量）的值都含有与原来对象的相同的值，那些引用其他对象的变量将指向新复制出来的新对象，而不指向原来的对象，简单地说，深拷贝不仅拷贝对象，而且还拷贝对象包含的引用所指向的对象。

**再简单的说就是浅拷贝只拷贝对象，不拷贝引用。深拷贝对象引用全拷贝**



# 拷贝的引入

## 	引用拷贝

创建一个指向对象的引用变量的拷贝。

```java
Student student = new Student("张三", 12);
Student copyStudent = student;
System.out.println(student);
System.out.println(copyStudent);
```

输出结果：

```java
com.rjxy.copy.Student@1b6d3586
com.rjxy.copy.Student@1b6d3586
```

分析：从结果可以看出，它们输出的地址值是相同的，那么它们肯定是同一个对象。`new Student("张三",12)` 时在堆区创建了，而变量`student`和`copyStudent`只是引用而已，它们都指向了同一个对象。这就叫做引用拷贝。

例1  图解：

![image-20220724003033672](https://graphicbed.oss-cn-shanghai.aliyuncs.com/myBlog/image-20220724003033672.png)

## 对象拷贝

创建对象本身的一个副本。

例2：

```java
Student student = new Student("张三", 12);
Student cloneStudent = (Student) student.clone();
System.out.println(student);
System.out.println(cloneStudent);
```

输出结果

```
com.rjxy.copy.Student@1b6d3586
com.rjxy.copy.Student@4554617c
```

分析：由输出结果可以看出，它们的地址是不同的，也就是说创建了新的对象， 而不是把原对象的地址赋给了一个新的引用变量,这就叫做对象拷贝。

![image-20220724003515997](https://graphicbed.oss-cn-shanghai.aliyuncs.com/myBlog/image-20220724003515997.png)

**注：深拷贝和浅拷贝都是对象拷贝**



# 浅拷贝

浅拷贝仅仅复制所考虑的对象，而不复制它所引用的对象。

## 实现Cloneable重写clone

`注：此方法默认是浅克隆`

<span id="three">例3：</span>

定义两个类`Teacher`和`Student`

Teacher类：

```java
class Teacher implements Cloneable {
    String name;

    public Teacher(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Teacher{" +
            "name='" + name + '\'' +
            '}';
    }
}
```

Student类：

```java
class Student implements Cloneable {
    String name;
    Integer id;
    Teacher teacher;

    public Student(String name, Integer id, Teacher teacher) {
        this.name = name;
        this.id = id;
        this.teacher = teacher;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    @Override
    public String toString() {
        return "Student{" +
            "name='" + name + '\'' +
            ", id=" + id +
            ", teacher=" + teacher +
            '}';
    }
}
```

测试：

```java
public class CopyTest {
        public static void main(String[] args) throws Exception {
            Teacher teacher = new Teacher("李四老师");
            Student student = new Student("张三", 12, teacher);
            Student cloneStudent = (Student) student.clone();
            System.out.println(student);
            System.out.println(cloneStudent);
            teacher.name = "王五老师";
            System.out.println("=========修改老师信息后=========");
            System.out.println(student);
            System.out.println(cloneStudent);
        }
    }
}
```

输出结果：

```
Student{name='张三', id=12, teacher=Teacher{name='李四老师'}}
Student{name='张三', id=12, teacher=Teacher{name='李四老师'}}
=========修改老师信息后=========
Student{name='张三', id=12, teacher=Teacher{name='王五老师'}}
Student{name='张三', id=12, teacher=Teacher{name='王五老师'}}
```

[例3 ](#three)图解：

![image-20220724005719719](https://graphicbed.oss-cn-shanghai.aliyuncs.com/myBlog/image-20220724005719719.png)

分析：

​	两个`student`和`cloneStudent`指向不同的对象，但是两个引用`student`和`copyStudent`中的`teacher`引用指向的是同一个对象，所以说是浅拷贝。



# 深拷贝

深拷贝是把要复制的对象所引用的对象都复制一遍。

<span id="demo">例4：</span>

Teacher类：

```java
class Teacher implements Cloneable {
    String name;

    public Teacher(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Teacher{" +
            "name='" + name + '\'' +
            '}';
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

Student类：

```java
class Student implements Cloneable {
    String name;
    Integer id;
    Teacher teacher;

    public Student(String name, Integer id, Teacher teacher) {
        this.name = name;
        this.id = id;
        this.teacher = teacher;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        // 浅复制时
        // return super.clone();

        //深复制时
        Student student = (Student) super.clone();
        student.teacher = (Teacher) student.teacher.clone();
        return student;
    }

    @Override
    public String toString() {
        return "Student{" +
            "name='" + name + '\'' +
            ", id=" + id +
            ", teacher=" + teacher +
            '}';
    }
}
```

测试代码：

```java
Teacher teacher1 = new Teacher("李四老师");
Student student = new Student("张三", 12, teacher1);
Student cloneStudent = (Student) student.clone();
System.out.println(student);
System.out.println(cloneStudent);

teacher.name = "王五老师";
System.out.println("=========修改老师信息后=========");
System.out.println(student);
System.out.println(cloneStudent);
```

结果：

```
Student{name='张三', id=12, teacher=Teacher{name='李四老师'}}
Student{name='张三', id=12, teacher=Teacher{name='李四老师'}}
=========修改老师信息后=========
Student{name='张三', id=12, teacher=Teacher{name='王五老师'}}
Student{name='张三', id=12, teacher=Teacher{name='李四老师'}}
```

结果分析：

&emsp;&emsp;两个引用`student`和`copyStudent` 指向不同的对象，这两个对象中的成员变量`teacher`是指向的两个对象，对`teacher1`修改只能影响到`student`对象，所以说是深拷贝。

[例4](#demo) 图解（teacher姓名更改前）

![image-20220724103345731](https://graphicbed.oss-cn-shanghai.aliyuncs.com/myBlog/image-20220724103345731.png)

[例4](#demo) 图解（teacher姓名更改后）

![image-20220724103431359](https://graphicbed.oss-cn-shanghai.aliyuncs.com/myBlog/image-20220724103431359.png)



# 深拷贝的几种方式

1.[clone()实现](#demo)

2.构造函数实现

```java
Teacher teacher = new Teacher("李四老师");
Student student = new Student("张三", 12, teacher);
Student cloneStudent = new Student("小二", 11, new Teacher(teacher.name));
System.out.println(student);
System.out.println(cloneStudent);

teacher.name = "王五老师";
System.out.println("=========修改老师信息后=========");
System.out.println(student);
System.out.println(cloneStudent);
```

3.序列化

```java
public Object deepClone() throws Exception {
    // 序列化
    ByteArrayOutputStream bos = new ByteArrayOutputStream();
    ObjectOutputStream oos = new ObjectOutputStream(bos);
    oos.writeObject(this);

    // 反序列化
    ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
    ObjectInputStream ois = new ObjectInputStream(bis);
    return ois.readObject();
}
```

4.Apache的Commons-lang

注：<font color="red">**对象需要实现Serializable接口**</font>

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.1</version>
</dependency>
```

```java
public Student deepClone2() {
    return SerializationUtils.clone(this);
}
```

5.Jackson的JSON序列化

注：<font color="red">**使用Jackson的JSON序列化时要为被序列化类加上`get`和`set`方法，以及`无参构造`**</font>

```java
public Student deepClone3() throws Exception {
    ObjectMapper objectMapper = new ObjectMapper();
    return objectMapper.readValue(objectMapper.writeValueAsString(this), Student.class);
}
```

6.Gson的JSON序列化

7.FastJson的JSON序列化

💻参考自[Java深入 深拷贝与浅拷贝详解](https://baiyexing.blog.csdn.net/article/details/71788741?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-71788741-blog-103269326.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-71788741-blog-103269326.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=1)











































