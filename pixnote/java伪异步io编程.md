---
title: (学习netty)java伪异步io编程
tags:
  - netty
  - socket
  - 学习笔记
date: 2018-10-11 15:30:59
cover_picture: https://ws4.sinaimg.cn/large/006tNbRwgy1fw447bl60pj308u04kgli.jpg
categories: netty
---

![timg ](https://ws4.sinaimg.cn/large/006tNbRwgy1fw447bl60pj308u04kgli.jpg)

# 伪异步I/O编程

参考：图书《Netty权威指南》

​	为了解决同步阻塞I/O棉铃的一个链路需要一个线程处理的问题。人们对线程模型进行优化。

服务器端通过一个县城池来处理多个客户端的请求接入，形成客户端个数M,线程池最大连接数N的比例关系，M远大于N。

通过线程池可以领过的分配线程资源，设置线程的最大值，防止由于海量并发接入导致线程耗尽。

## 1、伪异步I/O模型图

![屏幕快照 2018-10-11 下午3.34.39](https://ws2.sinaimg.cn/large/006tNbRwly1fw4blvguoaj31620eewv9.jpg)



用线程池来控制客户端数量，避免资源耗尽。

范例：采用时间服务器示例，完成伪异步I/O模型。

## 2、伪异步I/O服务器

代码Demo:https://github.com/IcyHeartBX/nettydemo/tree/master/NettyJavaBioDemo

- `BioTimeServer2.java`

```java
package com.pix.javabio.bio;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Date;
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

/**
 * 伪异步io实现时间服务器，采用线程池
 * 客户端发送"QUERY TIME ORDER"指令，服务器返回当前时间
 */
public class BioTimeServer2 {
    private final static  String TAG = "BioTimeServer2";
    private static final int PORT = 8899;
    private ServerSocket serverSocket;
    public void startServer() {
        System.out.println("CLASS "+ TAG +",startServer(),port:" + PORT);
        try {
            serverSocket = new ServerSocket(PORT);
            Socket socket = null;
            // 创建线程池
            TimeServerHandlerExecutePool singleExecutor
                    = new TimeServerHandlerExecutePool(50,10000);
            while(true) {
                // 接收客户端请求
                socket = serverSocket.accept();
                // 线程池处理
                singleExecutor.execute(new TimeServerHandler(socket));

            }
        }
        catch (Exception e) {
            e.printStackTrace();
        }
        finally {
            if(null != serverSocket) {
                System.out.println("CLASS " + TAG + ",server closed!");
                try {
                    serverSocket.close();
                    serverSocket = null;
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
    /**
     * 服务器客户端连接处理
     */
    private class TimeServerHandler implements Runnable{
        private Socket socket;
        public TimeServerHandler(Socket socket) {
            this.socket = socket;
        }
        @Override
        public void run() {
            BufferedReader in = null;
            PrintWriter out = null;
            try {
                // 获取输入流
                in = new BufferedReader(new InputStreamReader(this.socket.getInputStream()));
                // 获取输出流
                out = new PrintWriter(this.socket.getOutputStream(),true);
                String currentTime = null;
                String order = null;
                while(true) {
                    order = in.readLine();
                    if(null == order) {
                        return ;
                    }
                    System.out.println("CLASS " + TAG + ",receiv client order:" + order);
                    currentTime = "QUERY TIME ORDER".equalsIgnoreCase(order)
                            ?new Date(System.currentTimeMillis()).toString()
                            : "BAD ORDER";
                    // 写出时间
                    out.println(currentTime);
                }

            } catch (IOException e) {
                e.printStackTrace();
            }
            finally {
                try {
                    if(null != in) {
                        in.close();
                        in = null;
                    }
                    if(null != out) {
                        out.close();
                        out = null;
                    }
                    if(null != this.socket) {
                        this.socket.close();
                        this.socket = null;
                    }

                }
                catch (IOException e) {
                    e.printStackTrace();
                }
            }

        }
    }

    /**
     * 时间服务器执行线程池
     */
    private class TimeServerHandlerExecutePool {
        private ExecutorService executorService;
        public TimeServerHandlerExecutePool(int maxPoolSize,int queueSize) {
            executorService = new ThreadPoolExecutor(Runtime.getRuntime().availableProcessors()
            ,maxPoolSize,120L, TimeUnit.SECONDS,new ArrayBlockingQueue<Runnable>(queueSize));
        }
        public void execute(Runnable task) {
            executorService.execute(task);
        }
    }
}

```

伪异步I/O服务器类发生了变化，创建一个时间服务器处理类的线程池，当收到客户端连接时，将请求Socket封装成Task,然后调用线程池的execute方法执行，从而避免每个请求都手动创建一个线程的问题。

由于线程池消息队列都是有界限的。当客户端数目增多时，能够有效防止内存溢出，资源耗尽的问题。相对于传统的线程模型，是一种改良。

## 3、伪异步I/O服务器的弊端

当对Socket的输入流进行读取操作的时候，它会一直阻塞下去，直到发生如下三种事件：

- 有数据可读
- 可用数据已经读取完毕
- 发生空指针或I/O异常

这会造成请求或者应答消息缓慢，网络缓慢造成长时间阻塞。

`OutputStream`的`write()`方法写输出流的时候，它将被阻塞，直到所有字节全部写完，或者发生异常。



异步I/O可能造成的故障：

- 服务器处理缓慢，返回应答消息耗费60s,平时要10s
- 采用伪异步I/O的线程正在读取故障服务器节点的响应，由于读取输入流是阻塞的，它将被同步阻塞60s.
- 假如所有的可用线程都被故障服务器阻塞，那后续所有的I/O消息都将在队列中排队。
- 由于线程池采用阻塞队列实现，当队列积满之后，后续入队列的操作将被阻塞。
- 由于前端只有一个Aceptor线程接收客户端接入，他被阻塞在线程池的同步阻塞队列之后，新的客户端请求消息将被拒绝，客户端发生大量的连接超时。
- 由于几乎所有的连接都超时，会认为系统已经崩溃，无法接收新的请求消息。





























