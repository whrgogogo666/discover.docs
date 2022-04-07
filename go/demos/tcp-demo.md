# 基于go的tcp编程案例

> 关于tcp编程   

TCP/IP(Transmission Control Protocol/Internet Protocol) 即传输控制协议/网间协议;
是一种面向连接（连接导向）的、可靠的、基于字节流的传输层（Transport layer）通信协议;
因为是面向连接的协议，数据像水流一样传输，会存在黏包问题。

基于tcp的编程实际上就是socket编程。基于tcp的socket编程就是基于端口监听的server下，
实现client和server端进行连接并进行数据通信的一种通信方式。

而Go语言包中处理 UDP Socket 和 TCP Socket 不同的地方就是在服务器端处理多个客户端请求数据包的方式不同，
UDP 缺少了对客户端连接请求的 Accept 函数，其他基本几乎一模一样，只有 TCP 换成了 UDP 而已。

> 实现的功能  

本案例实现的功能就是：基于tcp的socket编程实例，启动一个server端，并监听多client端的连接，
实现多client端和server端的实时通信。

> 如何实现   

go语言提供了丰富的内置的网络编程的接口-net包。


> server端   

```go
package tcp_chapter

import (
	"bufio"
	"fmt"
	"net"
	"os"
	"strings"
)

func processConn(conn net.Conn) {
	defer conn.Close()

	// 通信的时候也需要进行循环，这样可以连续进行数据的接收
	reader := bufio.NewReader(os.Stdin)
	for {
		var tmp [128]byte
		n, err := conn.Read(tmp[:]) //将tmp转为切片
		if err != nil {
			fmt.Println("read from conn failed: ", err)
			return
		}
		// n表示客户端传递给服务短数据的长度
		fmt.Printf("server: server received data:%s, data_size:%d. \n", string(tmp[:]), n)

		// 给客户端传递信息
		fmt.Println("请回复：")
		msg, _ := reader.ReadString('\n')
		msg = strings.TrimSpace(msg)
		if msg == "exit" {
			break
		}
		conn.Write([]byte(msg))
	}

}

func Server() {
	// 本地端口启动服务
	listener, err := net.Listen("tcp", "127.0.0.1:20000")
	if err != nil {
		fmt.Println("start tcp server on 127.0.0.1:20000 error...", err)
		return
	}

	// 循环等待连接
	for {
		fmt.Println("server: server is listening...")

		// 等待别人来跟我建立链接
		conn, err := listener.Accept() //如果没有client进行连接，就会在此阻塞
		if err != nil {
			fmt.Println("accept failed, err:", err)
			return
		}

		// 与客户端通信
		go processConn(conn)
	}

}

```



> client端  

```go
package tcp_chapter

import (
	"bufio"
	"fmt"
	"net"
	"os"
	"strings"
)

// tcp client

func Client() {
	// 与server 端建立链接
	conn, err := net.Dial("tcp", "127.0.0.1:20000") //dial: 拨号
	if err != nil {
		fmt.Println("dial error: ", err)
		return
	}

	// 和服务其端通信
	reader := bufio.NewReader(os.Stdin)
	for {
		// 发送信息
		fmt.Println("请输入：")
		msg, _ := reader.ReadString('\n') //读到换行结束
		msg = strings.TrimSpace(msg)
		if msg == "exit" {
			break
		}
		conn.Write([]byte(msg))

		// 读取信息
		var tmp [128]byte
		n, err := conn.Read(tmp[:]) //将tmp转为切片
		if err != nil {
			fmt.Println("read from conn failed: ", err)
			return
		}
		// n表示客户端传递给服务短数据的长度
		fmt.Printf("client: client received data:%s, data_size:%d. \n", string(tmp[:]), n)
	}
	defer conn.Close()
}

```


> 实现结果展示   
- _启动服务端进行端口监听_    
`server: server is listening...  `   
`server: server is listening... `      
`server: server received data:你好, data_size:6.    `  
`请回复：   `   
`world   `  

- _开启多个客户端进行通信_   
`请输入：`     
`你好   `  
`client: client received data:world, data_size:5. `  


  


