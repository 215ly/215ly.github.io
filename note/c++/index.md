# [VisualStudio官网下载地址](https://visualstudio.microsoft.com/zh-hans/vs/) 



# 网络编程
## 1.socket()函数
`int socket (int __domain, int __type, int __protocol);`

### 参数列表
-  __domain：为协议域，又称协议族，我们最常用的有AF_INET、AF_INET6（也可以写作为PF_INET、PF_INET6），分别代表IPv4地址和IPv6地址。

2. __type：为数据传输方式或套接字类型，最常见的有SOCK_STREAM和 SOCK_DGRAM，其中SOCK_STREAM为面向连接的数据传输方式，是基于TCP的协议，数据可以准确无误地到达另一台计算机，如果损坏或丢失，可以重新发送，但效率相对较慢；而SOCK_DGRAM是无连接的数据传输方式，是基于UDP的协议，即只管传输数据，不作数据校验，如果数据在传输中损坏，或者没有到达另一台计算机，是没有办法补救的。

3. __protocol：为传输协议，对应上述的__type，常用的有IPPROTO_TCP 和 IPPTOTO_UDP分别代表TCP和UDP协议。而系统会根据__type的值自行选择，因此该项一般可直接指定为0。

具体使用方法
`int serv_sock = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);`

# 程序中一般可以打开多少个socket？
> 单个线程缺省条件下1024个（可以由系统参数进行修改），`系统默认打开文件数`