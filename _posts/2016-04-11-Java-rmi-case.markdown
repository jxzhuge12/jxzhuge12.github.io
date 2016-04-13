---
layout:     post
title:      "史上最简单的 Java RMI 教程"
date:       2016-4-11 12:00:00
author:     "jxzhuge12"
catalog:    true
latex:      false
tags:
    - Projects
    - Java
---

## Java RMI 介绍

Java 远程方法调用，即 **Java RMI (Java Remote Method Invocation)** 是 Java 编程语言里，一种用于实现远程过程调用的应用程序编程接口。它使客户机上运行的程序可以调用远程服务器上的对象。远程方法调用特性使 Java 编程人员能够在网络环境中分布操作。RMI全部的宗旨就是尽可能简化远程接口对象的使用。

Java RMI 极大地依赖于接口。在需要创建一个远程对象的时候，程序员通过传递一个接口来隐藏底层的实现细节。客户端得到的远程对象句柄正好与本地的根代码连接，由后者负责透过网络通信。这样一来，程序员只需关心如何通过自己的接口句柄发送消息。

## Java RMI 教程

> 运行环境：Ubuntu 15.10 + Java 8

**在 Desktop 目录新建文件夹并命名为 rmitest。本教程所有代码都在该文件夹下。但是，所有 java 指令都在 Desktop 目录运行。**

本教程分4步：

* [interface 及其实现]({{ page.url }}/#interface-)
* [Server 端]({{ page.url }}/#server-)
* [Client 端]({{ page.url }}/#client-)
* [运行]({{ page.url }}/#section)

#### interface 及其实现

~~~ java
package rmitest;

import java.rmi.Remote;
import java.rmi.RemoteException;

//File name: Calculator.java
public interface Calculator extends Remote {
    int add(int a, int b) throws RemoteException;
    int sub(int a, int b) throws RemoteException;
    int mul(int a, int b) throws RemoteException;
    int div(int a, int b) throws RemoteException;
}
~~~

~~~ java
package rmitest;

import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;

//File name: CalculatorImpl.java
public class CalculatorImpl extends UnicastRemoteObject implements Calculator {
    public CalculatorImpl() throws RemoteException {
        super();
    }

    public int add(int a, int b) throws RemoteException {
        return a + b;
    }

    public int sub(int a, int b) throws RemoteException {
        return a - b;
    }

    public int mul(int a, int b) throws RemoteException {
        return a * b;
    }

    public int div(int a, int b) throws RemoteException {
        return a / b;
    }
}
~~~

上述两个文件在 **rmitest** 文件夹中保存好后，回到 **Desktop** 目录，运行

~~~
javac rmitest/*.java
rmic rmitest.CalculatorImpl
~~~

> Note: 在编译之前一定不能把 server 和 client 的代码也放到文件夹里！

#### Server 端

~~~ java
package rmitest;

import java.net.MalformedURLException;
import java.rmi.AlreadyBoundException;
import java.rmi.Naming;
import java.rmi.RemoteException;
import java.rmi.registry.LocateRegistry;

import rmitest.CalculatorImpl;

//File name: CalculatorServer.java
public class CalculatorServer {
    public static void main(String[] args) {
        try {
            Naming.bind("rmi://localhost:1099/CalculatorServer", new CalculatorImpl());
        } catch (RemoteException e) {
            e.printStackTrace();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (AlreadyBoundException e) {
            e.printStackTrace();
        }
    }
}
~~~

上述文件在 **rmitest** 文件夹中保存好后，回到 **Desktop** 目录，运行

~~~
javac rmitest/CalculatorServer.java
~~~

#### Client 端

~~~ java
package rmitest;

import java.net.MalformedURLException;
import java.rmi.Naming;
import java.rmi.NotBoundException;
import java.rmi.RemoteException;

import rmitest.Calculator;

//File name: CalculatorClient.java
public class CalculatorClient {
    public static void main(String[] args) {
        try {
            Calculator calculator = (Calculator) Naming.lookup("rmi://localhost:1099/CalculatorServer");
            System.out.println(calculator.add(1, 1));
            System.out.println(calculator.sub(1, 1));
            System.out.println(calculator.mul(1, 1));
            System.out.println(calculator.div(1, 1));
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (RemoteException e) {
            e.printStackTrace();
        } catch (NotBoundException e) {
            e.printStackTrace();
        }
    }
}
~~~

上述文件在 **rmitest** 文件夹中保存好后，回到 **Desktop** 目录，运行

~~~
javac rmitest/CalculatorClient.java
~~~

#### 运行

打开三个 **Command** ，都进入到 **Desktop** 目录下。

在第一个 **Command** 中运行

~~~
rmiregistry
~~~

在第二个 **Command** 中运行

~~~
java rmitest.CalculatorServer
~~~

在第三个 **Command** 中运行

~~~
java rmitest.CalculatorClient
~~~

就可以看到结果了。

## 致谢

<a target="_blank" href="https://zh.wikipedia.org/wiki/Java%E8%BF%9C%E7%A8%8B%E6%96%B9%E6%B3%95%E8%B0%83%E7%94%A8">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-wikipedia-w fa-stack-1x fa-inverse"></i>
    </span>
</a>[Java 远程方法调用](https://zh.wikipedia.org/wiki/Java%E8%BF%9C%E7%A8%8B%E6%96%B9%E6%B3%95%E8%B0%83%E7%94%A8)

<a target="_blank" href="http://www.cnblogs.com/javalouvre/p/3726256.html">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-stack-1x fa-inverse">B</i>
    </span>
</a>[RMI 入门教程](http://www.cnblogs.com/javalouvre/p/3726256.html)
