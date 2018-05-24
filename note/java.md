---
layout: page
title: Java 查漏补缺
tags: [note]
---

## try-with-resource

try-with-resource 语句块可以用来自动关闭资源。我们先来看一下没有用这个语句块的实现：

~~~java
public InputStream getInputStream(File file) throwsIOException {
    InputStream inputStream = null;
    try {
        inputStream = new FileInputStream(file);
    } finally {
        if (inputStream != null) {
            inputStream.close();
        }
    }
    return inputStream;
}
~~~

可以看到，我们需要在 finally 语句中手动的关闭输入流。如果忘记这一步操作，那输入流可能就永远不会被关闭了。

我们再来看一下用 try-with-resource 的实现：

~~~java
public InputStream tryWithResource(File file) throws IOException {
    try (InputStream inputStream = new FileInputStream(file)) {
        return inputStream;
    }
}
~~~

在这一段代码里面已经见不到任何释放资源的操作了，事实上，编译器在编译代码时会帮我们补上这个操作。代码变得更加的简洁优美了，在需要同时释放多个资源的情况下更明显。

## 并发

### 原子性
原子性：即一个操作或者多个操作 要么全部执行并且执行的过程不会被任何因素打断，要么就都不执行。

Java内存模型只保证了基本读取和赋值是原子性操作。

### 可见性

线程1对变量i修改了之后，线程2没有立即看到线程1修改的值。这就是可见性问题。

对于可见性，Java提供了volatile关键字来保证可见性。

当一个共享变量被volatile修饰时，它会保证修改的值会立即被更新到主存，当有其他线程需要读取时，它会去内存中读取新值。

### 有序性

编译器的指令重排可能导致有序性问题。编译器和处理器都存在指令重拍，指令重拍的用意是提高性能。可以这么说，为了保证代码执行的有序性，我们是要做出性能的牺牲的。

## JDK

- 64位的 JDK 相比32位会有一定的性能损失，主要来源于本地指针的额外开销。详细的解释：[64bit_performance](http://www.oracle.com/technetwork/java/hotspotfaq-138619.html#64bit_performance)