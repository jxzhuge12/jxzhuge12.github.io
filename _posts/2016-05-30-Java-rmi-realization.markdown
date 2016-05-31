---
layout:     post
title:      "Java RMI 的具体实现"
date:       2016-5-30 12:00:00
author:     "jxzhuge12"
catalog:    true
latex:      false
tags:
    - Projects
    - Java
---

## Introduction

在 [史上最简单的 Java RMI 教程](http://jxzhuge12.me/2016/04/11/Java-rmi-case/) 中介绍了 Java RMI 的使用样例，在这篇 Blog 里，我将会介绍如何通过使用 Java Reflection 来实现 Java RMI 的功能。

## Java RMI 工作流程

Java RMI 的简化实现流程如下图所示。

<img src="/img/in-post/rmi_implementation.png" width="520">

1. [通过 Stub 创建 Instance]({{ page.url }}/#create-a-new-instance)
2. [调用 Instance 所拥有的 methods]({{ page.url }}/#invoke-methods)
3. [Instance 通过 Stub 和 Skeleton 传递参数至 Server]({{ page.url }}/#get-parameters)
4. [Server 接收参数并运行 methods]({{ page.url }}/#run)
5. [得到结果并通过 Skeleton 和 Stub 传回 Client]({{ page.url }}/#transfer-result)
6. [Client 接收结果]({{ page.url }}/#accept-result) 

#### Create a New Instance

在 Client 端创建 public abstract class Stub，并且通过 create method 调用以下函数来创造 new Instance。

~~~ java
java.lang.Object java.lang.reflect.Proxy.newProxyInstance(ClassLoader loader,
              Class<?>[] interfaces,
              InvocationHandler h)
                      throws IllegalArgumentException
~~~

#### Invoke Methods

在通过 newProxyInstance 构造的 new Instance 里，我们需要实现 InvocationHandler class 中的 invoke method 来进行所有 method invocation 的处理。

InvocationHandler 的具体框架如下：

~~~ java
public class MyInvocationHandler implements InvocationHandler, Serializable
{
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {}
}
~~~

我们每次对 Instance 的 method 的调用都会通过 InvocationHandler 的 invoke method 实现。具体的 method 以及参数都存放在 Method method 和 Object\[\] args 中。

然后通过 java.io.ObjectOutputStream，我们就可以对 Server 端传递 method name 和参数 Object\[\] args 了。

#### Get Parameters

在 Server 端，我们需要设置 Listener thread。每当有 Client 接入时，Listener 线程需要 create new thread 并且通过 java.io.ObjectInputStream 来接收 method name 和 Object\[\] args。

#### Run

通过 method name，我们就可以找到指定的 method 并且通过 Method.invoke 来运行。

~~~ java
java.lang.Object Method.invoke(Object obj,
            Object... args)
              throws IllegalAccessException,
                     IllegalArgumentException,
                     InvocationTargetException
~~~

#### Transfer Result

在 Server 端通过 java.io.ObjectOutputStream 对结果进行传递。

#### Accept Result

在 Client 端通过 java.io.ObjectInputStream 对结果进行接收。

## Glitches

在这个 project 中，我们遇到过以下问题：

* [重载函数]({{ page.url }}/#overloaded-function)
* [equals method 的本地实现]({{ page.url }}/#equals-method-local-realization)

#### overloaded function

在 Client 端传递 method name 和 Object\[\] args 到 Server 端时，由于我们不知道 Object\[\] args 的具体类型，导致可能对 overloaded function 进行选择的时候会出错。

我们通过调用 Method.getParameterTypes() 来获得所有参数类型，并且将 method name 以及所有参数类型拼接成字符串，再将该字符串的 hashcode 传递到 Server 端进行匹配。

#### equals method local realization

由于 equals method 的特殊性，我们在 Client 端直接实现。最初，我们在 InvocationHandler 接收到的 method name 为 equals 的时候就进入 equal 的判断。但是这样带来了别的问题：equals method 被重载过。

所以，我们判断 Object\[\] args 的长度为1，并且参数类型为 java.lang.Object 的时候进入 equal 判断。