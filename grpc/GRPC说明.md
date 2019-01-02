# GRPC的说明 #

> 在gRPC中，客户端应用程序可以直接在不同机器上的服务器应用程序上调用方法，就像它是本地对象一样，使您更容易创建分布式应用程序和服务。 与许多RPC系统一样，gRPC基于定义服务的思想，指定可以使用其参数和返回类型远程调用的方法。 在服务器端，服务器实现此接口并运行gRPC服务器来处理客户端调用。 在客户端，客户端有一个stub（简称为一些语言的客户端），提供与服务器相同的方法。gRPC客户端和服务器可以在各种环境中运行和交互，从Google内部的服务器到您自己的应用，并且可以使用任何gRPC支持的语言编写。 所以，例如，您可以轻松地创建一个Java开发的服务，使用Go，Python或Ruby中的客户端。In addition, the latest Google APIs will have gRPC versions of their interfaces, letting you easily build Google functionality(功能） into your applications。

## 服务定义 ##

> 像许多RPC系统一样，gRPC基于定义服务的思想，指定可以使用其参数和返回类型远程调用的方法。 默认情况下，gRPC使用 protocol buffers作为接口定义语言（IDL），用于描述有效负载消息的服务接口和结构。

```
service HelloWorldService {
  rpc SayHello (HelloWorldRequest) returns (HelloWorldResponse);
}

message HelloWorldRequest {
  string greeting = 1;
}

message HelloWorldResponse {
  string reply = 1;
}
```

## gRPC定定义的四种服务接口 ##

### 一元定义 ###

> 客户端向服务器发送请求并获得响应，就像正常的函数调用一样

```
rpc SayHello(HelloRequest) returns (HelloResponse){}
```

### 服务器流式RPC定义 ###

> 客户端发送一个对象服务器端返回一个Stream（流式消息）

```
rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse){}
```

### 客户端流式RPC定义 ###

> 客户端发送一个Stream（流式消息）服务端返回一个对象

```
rpc LotsOfGreetings(stream HelloRequest) returns (HelloResponse) {}
```

### 双向流RPC定义 ###

> 其中双方使用读写流发送消息序列。 两个流独立运行，所以客户端和服务器可以按照他们喜欢的顺序进行读取和写入：例如，服务器可能在写入响应之前等待接收所有客户端消息，或者可以交替地读取消息然后写入消息， 或读取和写入的其他组合。 每个流中消息的顺序被保留。
类似于WebSocket（长连接），客户端可以向服务端请求消息，服务器端也可以向客户端请求消息）
```
rpc BidiHello(stream HelloRequest) returns (stream HelloResponse){}
```