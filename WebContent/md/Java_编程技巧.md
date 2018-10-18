# 用法

## 双括号初始化

“双括号初始化”（double brace initialization），用于创建匿名列表并添加元素。

```java
invite(new ArrayList<String>() {{ add(“Eric”); add(“Zong”) }});
```

说明：外层括号建立了ArrayList的一个匿名子类，内层括号则是一个对象构造块。

## 双层捕获关闭流 

```java
InputStream in = ...;
try
{
  try
  {
    // code that might throw exception
  }
  finally
  {
    in.close();
  }
}
catch(IOException e)
{
  // show error message
}
```

说明：内层的try语句块只有一个职责，就是确保关闭输入流。外层的try语句块也只有一个职责，就是确保报告出现的错误。这种设计方式不仅清楚，而且还具有一个功能，就是将会报告 finally 子句中出现的错误。

# 调试

## 命令行捕获流

捕获错误流：

Java MyProgram 2> errors.txt

同时捕获输出流和错误流：

Java MyProgram >& errors.txt

# 兼容性

## 可变参数

可以将已经存在且最后一个参数是数组的方法重新定义为可变参数的方法，而不会破坏任何已经存在的代码。

# 用户体验

## 启动速度较快的幻觉

确保包含main方法的类没有显式地引用其他的类。显示一个启动画面，通过调用Class.forName手工地加载其他的类。


