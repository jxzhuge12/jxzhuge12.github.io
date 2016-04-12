---
layout:     post
title:      "史上最简单的Java RMI教程"
date:       2016-4-11 12:00:00
author:     "jxzhuge12"
catalog:    true
latex:      false
tags:
    - Projects
    - Java
---

## Java RMI介绍

Java远程方法调用，即Java RMI（Java Remote Method Invocation）是Java编程语言里，一种用于实现远程过程调用的应用程序编程接口。它使客户机上运行的程序可以调用远程服务器上的对象。远程方法调用特性使Java编程人员能够在网络环境中分布操作。RMI全部的宗旨就是尽可能简化远程接口对象的使用。

Java RMI极大地依赖于接口。在需要创建一个远程对象的时候，程序员通过传递一个接口来隐藏底层的实现细节。客户端得到的远程对象句柄正好与本地的根代码连接，由后者负责透过网络通信。这样一来，程序员只需关心如何通过自己的接口句柄发送消息。

## Java RMI教程

> 运行环境：Ubuntu 15.10 + Java 8
>
> 在Desktop目录新建文件夹并命名为rmitest。本教程所有代码都在该文件夹下。但是，所有java指令都在Desktop目录运行。

本教程分4步：

* interface以及具体实现
* Server端
* Client端
* 运行

#### interface以及具体实现

~~~ java
package rmitest;

import java.rmi.RemoteException;

//File name: Calculator.java
public interface Calculator extends java.rmi.Remote {
    int add(int a, int b) throws RemoteException;
    int sub(int a, int b) throws RemoteException;
    int mul(int a, int b) throws RemoteException;
    int div(int a, int b) throws RemoteException;
}
~~~

~~~ java
package rmitest;

import java.rmi.RemoteException;

//File name: CalculatorImpl.java
public class CalculatorImpl extends java.rmi.server.UnicastRemoteObject implements Calculator {
    private static final long serialVersionUID = 3434060152387200042L;

    public CalculatorImpl() throws RemoteException {
        super();
    }

    @Override
    public int add(int a, int b) throws RemoteException {
        return a + b;
    }

    @Override
    public int sub(int a, int b) throws RemoteException {
        return a - b;
    }

    @Override
    public int mul(int a, int b) throws RemoteException {
        return a * b;
    }

    @Override
    public int div(int a, int b) throws RemoteException {
        return a / b;
    }
}
~~~

上述两个文件在rmitest文件夹中保存好后，退出到Desktop目录，运行

~~~
javac rmitest/*.java
rmic rmitest.CalculatorImpl
~~~

#### Server端

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
            LocateRegistry.createRegistry(1098);
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

上述文件在rmitest文件夹中保存好后，退出到Desktop目录，运行

~~~
javac rmitest/CalculatorServer.java
~~~

#### Client端

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

上述文件在rmitest文件夹中保存好后，退出到Desktop目录，运行

~~~
javac rmitest/CalculatorClient.java
~~~

#### 运行

打开三个command，都进入到Desktop目录下。

在第一个command中运行

~~~
rmiregistry
~~~

在第二个command中运行

~~~
java rmitest.CalculatorServer
~~~

在第三个command中运行

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
</a>[Java远程方法调用](https://zh.wikipedia.org/wiki/Java%E8%BF%9C%E7%A8%8B%E6%96%B9%E6%B3%95%E8%B0%83%E7%94%A8)

<a target="_blank" href="http://www.cnblogs.com/javalouvre/p/3726256.html">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-stack-1x fa-inverse">B</i>
    </span>
</a>[RMI入门教程](http://www.cnblogs.com/javalouvre/p/3726256.html)