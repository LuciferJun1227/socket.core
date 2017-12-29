﻿socket.core
===

This is a socket framework written based on C # standard, the interface design is simple, separate thread operation, does not affect the caller. Can be used in the net Framework 4.x.x / standard assembly, in the window (IOCP) / linux normal operation.
---

Install NuGet:
Package Manager: Install-Package socket.core
.Net CLI: dotnet add package socket.core
Paket CLI: paket add socket.core

Server socket.core.Server namespace, respectively, three modes push / pull / pack
Under the socket.core.Client namespace of the client, there are three modes of push / pull / pack

The main process and the corresponding methods and events introduced.
Note: connectId (guid) represents a connection object, data (byte []), success (bool)
  
* 1. Initialize socket (corresponding to the three modes)
> Instantiate the server class TcpPushServer / TcpPullServer / TcpPackServer
> Instantiate the client class TcpPushClient / TcpPullClient / TcpPackClient
* 2. Start monitoring / connecting server
> Server server.Start (port);
> Client client.Connect (ip, port);
* 3. Trigger the connection event
> Server server.OnAccept (connectId); Received a connection id, can be used to send, receive, close the tag
> Client client.OnAccept (success); Receives whether to connect to the server successfully
* 4. Send a message
> Server server.Send (connectId, data, offset, length);
> Client client.Send (data, offset, length);
* 5. Triggered receive events
> Server server.OnReceive (connectId, data);
> Client client.OnReceive (data);
* 6. Close the connection
> Server server.Close (connectId);
> Client client.Close ();
* 7. Trigger to close the connection event
> Server server.OnClose (connectId);
> Client client.OnClose ();


Three models introduction
* One: push
    > Will trigger the monitor event object OnReceive (connectId, data); the data immediately "pushed" to the application
* Two: pull
    > OnReceive (connectId, length), which tells the application how much data has been received. The application checks the length of the data. If it meets, it calls the Fetch (connectId, length) method of the component, Data "pulled" out
* Three: pack
    > pack The model component is a combination of the push and pull models. The application does not have to deal with subcontracts. The component guarantees that every application server.OnReceive (connectId, data) /client.OnReceive (data) event provides the application with a Complete data package
Note: The pack model component automatically adds a 4-byte (32-bit) header to each packet sent by the application. When the component receives the data, it is automatically packetized based on the header information. Each complete packet is sent to OnReceive The event is sent to the application
PACK header format
XXXXXXXXXXYYYYYYYYYYYYYYYYYYYYYY
The first 10 X bits are the header identification bits, which are used for data packet verification. The effective header identification value ranges from 0 to 1023 (0x3FF). When the header identification equals 0, the header is not checked. The last 22 bits of Y are length bits. Package length. The maximum valid packet length can not exceed 4194303 (0x3FFFFF) bytes (bytes), the application can be set by the TcpPackServer / TcpPackClient constructor parameter headerFlag
The company is located in:
The company is located in:
The company is located in:
2017/12/27


socket.core 
===

这是一个基于C# standard 写的socket框架，接口设计简单，单独线程运行,不影响调用方。可使用于net Framework 4.x.x/standard程序集，能在window(IOCP)/linux正常运行.
---

安装NuGet:  
Package Manager: Install-Package socket.core   
.Net CLI :dotnet add package socket.core      
Paket CLI:paket add socket.core         

服务端所在socket.core.Server命名空间下，分别为三种模式 push/pull/pack    
客户端所在socket.core.Client命名空间下，分别为三种模式 push/pull/pack    

主要流程与对应的方法和事件介绍.    
注:connectId(guid)代表着一个连接对象,data(byte[]),success(bool)   
  
* 1.初始化socket(对应的三种模式)    
	>实例化服务端类 TcpPushServer/TcpPullServer/TcpPackServer     
	>实例化客户端类 TcpPushClient/TcpPullClient/TcpPackClient    
* 2.启动监听/连接服务器   
	>服务端 server.Start(port);   
	>客户端 client.Connect(ip,port);   
* 3.触发连接事件   
	>服务端 server.OnAccept(connectId);		接收到一个连接id,可用他来发送，接收，关闭的标记   
	>客户端 client.OnAccept(success);		接收是否成功连接到服务器   
* 4.发送消息   
	>服务端 server.Send(connectId,data,offset,length);  
	>客户端 client.Send(data,offset,length);
* 5.触发接收事件  
	>服务端 server.OnReceive(connectId, data);  
	>客户端 client.OnReceive(data);  
* 6.关闭连接  
	>服务端 server.Close(connectId);   
	>客户端 client.Close();   
* 7.触发关闭连接事件   
	>服务端 server.OnClose(connectId);   
	>客户端 client.OnClose();    


三种模型简介   
* 一:push   
    >会触发监听事件对象OnReceive(connectId,data);把数据立马“推”给应用程序  
* 二:pull   
    >到数据时会触发监听事件对象OnReceive(connectId,length)，告诉应用程序当前已经接收到了多少数据，应用程序检查数据的长度，如果满足则调用组件的Fetch(connectId,length)方法，把需要的数据“拉”出来  
* 三:pack   
    >pack模型组件是push和pull模型的结合体，应用程序不必要处理分包/合包，组件保证每个server.OnReceive(connectId,data)/client.OnReceive(data)事件都向应用程序提供一个完整的数据包   
	注：pack模型组件会对应用程序发送的每个数据包自动加上4个字节(32位)的包头，组件接收到数据时，根据包头信息自动分包，每个完整的数据包通过OnReceive事件发送给应用程序   
	PACK包头格式   
	XXXXXXXXXXYYYYYYYYYYYYYYYYYYYYYY   
	前10位X为包头标识位，用于数据包校验，有效包头标识取值范围0~1023(0x3FF),当包头标识等于0时，不校验包头，后22位Y为长度位，记录包体长度。有效数据包最大长度不能超过4194303（0x3FFFFF）字节(byte),应用程序可以通过TcpPackServer/TcpPackClient构造函数参数headerFlag设置    
	  
	    
	  
	2017/12/27