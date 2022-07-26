---
title: Optional类的基本使用
description: 学习java8新增类之Optional
date: '2022/7/26 23:38'
cover: 'https://graphicbed.oss-cn-shanghai.aliyuncs.com/myBlog/165884969091.jpg'
tags:
  - Java8
categories:
  - Java
abbrlink: 14992
---
# Optional类的基本使用
# 概述

我们在编写代码的时候出现最多的就是空指针异常。所以在很多情况下我们都需要各种非空的判断。

```java
Student student = new Student();
if(student != null){
    System.out.println(student.getName());
}
```

尤其是对象中的属性还是一个对象的情况下。这样的判断会很多。

所以在`jkd8`中引入了`optional`，来判断空指针。

# 使用

## 创建对象

Optional就好像是一个包装类，可以把我们具体的数据封装Optional对象内部。然后我们使用`Optional`中封装好的方法操作封装进去的数据就可以很优雅的避免空指针异常了。

**使用Optional.ofNullable(T value)来获取Optional对象**

```java
public class Demo{
    public static void main(Stirng[] args){
        Student student = new Student();
        Optional<Student> optional = Optional.ofNullable(student);
    }
}
```

**使用Optional.of(T value)来创建Optional对象**

1. 如果传入的数据为空的话会抛出`NullPointerException`

   ![165884667227](https://graphicbed.oss-cn-shanghai.aliyuncs.com/myBlog/165884667227.png)

## 消费

使用函数`ifPresent()`如果被包装的对象为空，则不会执行后面的代码。

1. 使用非Lambda表达式

   ```java
   public static void main(String[] args) {
       Student student = getStudent();
       Optional<Student> optional = Optional.ofNullable(student);
       optional.ifPresent(new Consumer<Student>() {
           @Override
           public void accept(Student student) {
               System.out.println(student.getName());
           }
       });
   }
   ```

2. 使用Lambda表达式

   ```java
   public static void main(String[] args) {
       Student student = getStudent();
       Optional<Student> optional = Optional.ofNullable(student);
       optional.ifPresent(student1 -> System.out.println(student1.getName()));
   }
   ```

如果`student`对象为空则不会执行后面代码



## 安全获取值

如果我们期望安全的获获取值。不推荐使用`get()`方法获取，而是使用Optional提供的以下方法

- orElseGet

  获取数据并且设置数据为空时的默认值。如果数据不为空就能获取到该对象。如果为空则根据你传入的参数来创建对象作为默认值返回。

  非Lambda表达式：

  ```java
  Student student1 = optional.orElseGet(new Supplier<Student>() {
      @Override
      public Student get() {
          // 设置默认值
          return new Student();
      }
  });
  ```

  Lambda表达式：

  ```java
  Student student1 = optional.orElseGet(() -> new Student());
  // 或者
  Student student1 = optional.orElseGet(Student::new);
  ```

- orElseThrow

  获取数据，如果数据不为空就能获取到该数据。如果为空则根据你传入的参数来创建异常抛出。

  非Lambda表达式：

  ```java
  try {
      optional.orElseThrow(new Supplier<Throwable>() {
          @Override
          public Throwable get() {
              return new RuntimeException("数据为null");
          }
      });
  } catch (Throwable e) {
      e.printStackTrace();
  }
  ```

  Lambda表达式：

  ```java
  try {
      optional.orElseThrow(() -> new RuntimeException("数据为null"));
  } catch (Throwable e) {
      e.printStackTrace();
  }
  ```



## 过滤

我们可以使用`filter()`对数据进行过滤。如果原本的是是有数据的，但是不符合判断，也会变成一个无数据的`Optional对象`

非Lambda表达式
```java
Optional<Student> optional = Optional.ofNullable(getStudent());
optional.filter(new Predicate<Student>() {
    @Override
    public boolean test(Student student) 
        // 过滤条件
        return student.getAge() > 18;
    }
});
```

Lambda表达式（结合ifPresent() ）

```java
Optional<Student> optional = Optional.ofNullable(getStudent());
optional.filter(student -> student.getAge() > 18).ifPresent(student -> System.out.println(student.getName()));
```



## 判断

使用`isPresent()`方法进行是否存在数据的判断，如果为空返回值为false，如果不为空，返回值为true。

```java
Optional<Student> optional = Optional.ofNullable(getStudent());
if(optional.isPresent()){
    System.out.print(optional.get().getName());
}
```



## 数据转换

Optional还提供了map可以让我们进行数据的转换，并且转换得到的数据也是被Optional包装好的，保证了我们的数据安全。

```java
Optional<Student> optional = Optional.ofNullable(getStudent());
optional.map(Student::getBooks).ifPresent(books -> System.out.println(books));
```





👨‍🏫学习视频地址[三更草堂](https://www.bilibili.com/video/BV1Gh41187uR?p=45&vd_source=1d7243fd76a9514cb610ba9e2f567ff8)



















 





















