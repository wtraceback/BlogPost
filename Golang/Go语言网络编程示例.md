### 1. 简单示例
1. 以下是一个使用 Go 语言标准库 net 实现的简单的客户端和服务器端示例。
2. 服务器端监听本地的 8080 端口，并在接收到客户端连接后，向客户端发送一条欢迎消息。
3. 客户端通过 ```Dial``` 方法连接服务器，并接收服务器发送的欢迎消息。

服务器端代码：
```go
package main

import (
    "fmt"
    "net"
)

func main() {
    // 监听端口
    listener, err := net.Listen("tcp", "localhost:8080")
    if err!= nil {
        fmt.Println("服务器监听端口错误: ", err)
        return
    }
    defer listener.Close()

    fmt.Println("服务器已启动，正在等待连接...")

    for {
        // 接收客户端的连接
        coon, err := listener.Accept()
        if err!= nil {
            fmt.Println("接收客户端连接时发生的错误: ", err)
            continue
        }

        // 开启一个 goroutine 处理客户端连接
        go handleConnection(coon)
    }
}

func handleConnection(coon net.Conn) {
    defer coon.Close()

    fmt.Println("客户端已连接...")
    fmt.Println("客户端地址: ", coon.RemoteAddr().String())
    fmt.Println()

    // 向客户端发送欢迎消息
    message := "欢迎来到服务器"
    coon.Write([]byte(message))
}
```

客户端代码：
```go
package main

import (
    "fmt"
    "net"
)

func main() {
    // 连接服务器
    conn, err := net.Dial("tcp", "localhost:8080")
    if err!= nil {
        fmt.Println("错误的连接: ", err)
        return
    }
    defer conn.Close()

    // 读取服务器发送的欢迎消息
    buffer := make([]byte, 1024)
    n, err := conn.Read(buffer)
    if err!= nil {
        fmt.Println("读取错误: ", err)
        return
    }

    // 打印欢迎消息
    message := string(buffer[:n])
    fmt.Println("从服务器接收到的欢迎消息: ", message)
}
```


### 2. HTTP 请求和响应的示例
以下是一个使用 Go 语言标准库 net 实现的简单的客户端和服务器端示例，用于发送 HTTP 请求并接收 HTTP 响应。

服务器代码：
```go
package main

import (
     "fmt"
     "net"
     "bufio"
)

func main() {
     // 监听端口
     listener, err := net.Listen("tcp", "localhost:8080")
     if err!= nil {
          fmt.Println("无法监听端口：", err)
          return
     }
     defer listener.Close()
     fmt.Println("服务器已启动，等待客户端连接...")

     for {
          // 接受连接
          conn, err := listener.Accept()
          if err!= nil {
               fmt.Println("接收连接失败：", err)
               continue
          }

          // 处理连接
          go handleConnection(conn)
     }
}

func handleConnection(conn net.Conn) {
     defer conn.Close()

     // 读取请求数据
     reader := bufio.NewReader(conn)
     request, err := reader.ReadString('\n')      // 只读取请求行（不读取请求头和请求体）
     if err!= nil {
          fmt.Println("读取请求失败：", err)
          return
     }
     fmt.Println("收到请求：\n", request)

     // 响应客户端
     response := "HTTP/1.1 200 OK\r\nContent-Type: text/plain\r\n\r\nHello World"
     conn.Write([]byte(response))
     fmt.Println("发送响应：\n", response)
}
```

客户端代码：
```go
package main

import (
    "fmt"
    "net"
)

func main() {
    // 连接服务器
    conn, err := net.Dial("tcp", "127.0.0.1:8080")
    if err!= nil {
        fmt.Println("无法连接到服务器：", err)
        return
    }
    defer conn.Close()

    // 向服务器发送 http 请求信息
    request := "GET / HTTP/1.1\r\nHost: 127.0.0.1\r\n\r\n"
    conn.Write([]byte(request))
    fmt.Println("发送请求：\n", request)

    // 读取服务器发送过来的响应信息
    buffer := make([]byte, 1024)
    n, err := conn.Read(buffer)
    if err!= nil {
        fmt.Println("读取响应失败：", err)
        return
    }

    // 打印响应消息
    response := string(buffer[:n])
    fmt.Println("接收到响应：\n", response)
}
```

1. 在上述示例中，服务器端使用 ```net.Listen``` 方法监听本地的 8080 端口，并等待客户端连接。一旦有客户端连接，就会启动一个新的 ```goroutine``` 来处理连接。

2. 服务器端通过 ```bufio.NewReader``` 从连接中读取 HTTP 请求字符串，然后发送一个包含 HTTP 响应字符串的 TCP 连接。

3. 客户端使用 ```net.Dial``` 方法连接到服务器端的地址，然后发送一个包含 HTTP 请求字符串的 TCP 连接。接着，客户端从连接中读取服务器端返回的 HTTP 响应字符串，并打印到控制台上。

4. 请注意，这只是一个简单的示例，没有对 HTTP 请求进行解析和处理，也没有处理连接的错误和关闭连接的情况。在实际应用中，你可能需要使用更复杂的库，如 ```net/http``` 来处理 HTTP 请求和响应。


### 3. 标准库文档
[Go 语言标准库文档-中文版](https://studygolang.com/pkgdoc)