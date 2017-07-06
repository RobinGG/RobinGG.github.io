---
layout: page
title: Mockito 用法
---

### 创建一个 mock 对象

~~~java
public class DemoTest{
    @Mock
    private List mockedList;

    @Before
    private void setUp() {
        List mock = mock(List.class);
    }
}
~~~

### 定义 mock 对象的行为

~~~java
when(mockedList.get(0)).thenReturn("first");
when(mockedList.get(1)).thenThrow(new RuntimeException());

when(mockedList.get(anyInt())).thenReturn("element");
when(mockedList.contains(argThat(isValid()))).thenReturn("element");

doThrow(new RuntimeException()).when(mockedList).clear();

when(mock.someMethod("some arg"))
 .thenThrow(new RuntimeException())
 .thenReturn("foo");

~~~

### 验证 mock 对象的行为

~~~java
verify(mockedList).add("one");
verify(mockedList).clear();
verify(mockedList).get(0);
verify(mockedList).get(anyInt());


~~~

