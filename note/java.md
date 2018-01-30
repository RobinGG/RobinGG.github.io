---
layout: page
title: Java 查漏补缺
tags: [note]
---

## JNDI

### JNDI 是什么？

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

我们再来看一下用 try-with-resource 的实现：

~~~java
public InputStream tryWithResource(File file) throws IOException {
    try (InputStream inputStream = new FileInputStream(file)) {
        return inputStream;
    }
}
~~~

在这一段代码里面已经见不到任何释放资源的操作了，事实上，编译器在编译代码时会帮我们补上这个操作。代码变得更加的简洁优美了，在需要同时释放多个资源的情况下更明显。