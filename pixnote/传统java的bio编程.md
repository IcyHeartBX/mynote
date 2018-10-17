---
title: (学习netty)传统java的bio编程
tags:
  - netty
  - socket
  - 学习笔记
date: 2018-10-10 15:30:59
cover_picture: https://ws4.sinaimg.cn/large/006tNbRwgy1fw447bl60pj308u04kgli.jpg
categories: netty
---

![timg ](https://ws4.sinaimg.cn/large/006tNbRwgy1fw447bl60pj308u04kgli.jpg)

# 传统的BIO通信示例

参考：图书《Netty权威指南》

同步阻塞模型开发中:ServerSocket负责绑定IP地址，启动监听端口。Socket负责发起连接操作。

连接成功后双方通过输入和输出流进行同步阻塞式通信。

- 以时间服务器(TimeServer)为例，完成示例 ：

该模型的缺点：

- 缺乏弹性伸缩能力，当客户端并发访问量增加后，服务端的线程个数和客户端并发访问数呈1:1的正比例关系，

BIO通信模型图：

![屏幕快照 2018-10-10 下午4.54.36](https://ws3.sinaimg.cn/large/006tNbRwgy1fw38qlwhisj318k0fih0b.jpg)

采用BIO通信模型的服务器，通常由一个独立的Acceptor线程负责监控客户端的连接，他接收到客户端连接请求之后为每个客户端创建一个新的线程进行链路处理，处理完成之后，通过输出流返回应答给客户端，线程销毁。这就是典型的一请求一应答通信模型。

当客户端增加，并发量增加，线程数目急剧增加。

## 1、服务端示例

服务端接受客户端的连接，连接成功后，接收客户端发来的消息，如果是“QUERY TIME ORDER”则返回当前服务器时间，如果不是，就返回`“BAD ORDER”`

Demo:https://github.com/IcyHeartBX/nettydemo/tree/master/NettyJavaBioDemo

- `BioTimeSever.java`

```java
package com.pix.javabio.bio;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Date;

/**
 * 阻塞式时间服务器类
 * 客户端发送"QUERY TIME ORDER"指令，服务器返回当前时间
 */
public class BioTimeServer {
    private final static  String TAG = "BioTimeServer";
    private static final int PORT = 8899;
    private ServerSocket serverSocket;
    public void startServer() {
        System.out.println("CLASS BioTimeServer,startServer(),port:" + PORT);
        try {
            serverSocket = new ServerSocket(PORT);
            Socket socket = null;
            while(true) {
                // 接收客户端请求
                socket = serverSocket.accept();
                // 创建处理线程
                new Thread(new TimeServerHandler(socket)).start();
            }
        }
        catch (Exception e) {
            e.printStackTrace();
        }
        finally {

        }
    }

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
}

```

- `TimeServerMain.java`

```java
package com.pix.javabio;

import com.pix.javabio.bio.BioTimeServer;

public class TimeServerMain {
    public static void main(String [] args) {
        System.out.println("Time server start!");
        BioTimeServer server = new BioTimeServer();
        server.startServer();
    }
}

```



## 2、客户端示例

客户端连接服务器，并且发送`"QUERY TIME ORDER"`的指令

- `BioTimeClient.java`

```java
package com.pix.javabio.bio;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

/**
 * 阻塞式同步获取时间Tcp客户端
 * 通过发送"QUERY TIME ORDER"来获取服务器时间
 */
public class BioTimeClient {
    private static final String TAG = "BioTimeClient";
    private static final int PORT = 8899;
    private static final String ORDER = "QUERY TIME ORDER";

    public void startClient() {
        System.out.println("CLASS " + TAG + ",startClient(),prot:" + PORT);
        Socket socket = null;
        BufferedReader in = null;
        PrintWriter out = null;
        try {
            // 创建套接字
            socket = new Socket("127.0.0.1",PORT);
            // 输入流
            in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            // 输出流
            out = new PrintWriter(socket.getOutputStream(),true);
            // 发送查询指令
            out.println(ORDER);
            System.out.println("CLASS" + TAG + ",startClient(), send query order success!");
            // 读取服务器返回值
            String respond = in.readLine();
            System.out.println("CLASS" + TAG + ",startClient(), receive server msg:" + respond);
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
                if(null != socket) {
                    socket.close();
                    socket = null;
                }
            }
            catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

```

- `TimeClientMain.java`

```java
package com.pix.javabio;

import com.pix.javabio.bio.BioTimeClient;

public class TimeClientMain {
    public static void main(String [] args) {
        System.out.println("Time client start!");
        BioTimeClient client = new BioTimeClient();
        client.startClient();
    }
}

```

运行结果：

- server

```shell
Time server start!
CLASS BioTimeServer,startServer(),port:8899
CLASS BioTimeServer,receiv client order:QUERY TIME ORDER
```

- client

```shell
Time client start!
CLASS BioTimeClient,startClient(),prot:8899
CLASSBioTimeClient,startClient(), send query order success!
CLASSBioTimeClient,startClient(), receive server msg:Thu Oct 11 15:13:55 CST 2018

Process finished with exit code 0
```



## 3、总结

BIO主要问题在于每当有一个新的客户端请求接入时，服务器必须建立一个新的线程处理新接入的客户端链路，一个线程只能处理一个客户端连接。

在高性能服务器应用领域，往往需要面向成千上万个客户端的并发连接。

这种模型显然无法满足高性能，高并发的接入场景。

为了改进一个县城连接模型，后来演进出，**线程池**和**消息队列**实现1个或者多个线程处理N个客户端的模型。这种模型底层仍然蚕蛹同步阻塞I/O，所以称为“伪异步”。



























