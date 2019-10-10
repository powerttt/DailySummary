传统socket通信的演进。

#### BlockIng I/O模式下，主要缺点如下：

1. 只能用于小规模下多个socket通信，因为客户端socket每次连接请求后，**服务端ServerSocket**都会**创建**一个**线程**来处理当前客户端的连接**请求**，如果连接数非常大，以千万级为单位，那么服务端的CPU资源开销会是一个非常庞大的数据。
2. Read、Write读写资源问题，由于是阻塞的读写模式，如果大量线程处于空闲状态没有数据可读写，则会造成**空闲**socket的**Read** 、**Write**操作大量**阻塞**，对系统资源线程的开销也会造成非常大的浪费。



#### NIO（not Blocking I/O ，也有人叫他new IO）

NIO区别与IO的时在他通过选择器和采用观察者模式将之前大量连接采用一个线程即可搞定。

同时通过通道的方式，可对流量进行重复读取

![NIO流程](F:\opt\githubproject\powerttt\daily-summary\2019年10月10日\Java-Socket\NIO.png)

##### NIO模式下下的优点：

1. NIO使用Channel和selector结合的方式，可以多次从通过读写或者写入通道数据，并且可以读取指定为的数据，较传统IO的方式采取的是流，一打开必须读到尾，不能读到指定位置的数据。

2. socketor选择器，在通过socketChannl将socket注册到选择器中，那么就可以通过一个线程处理注册进来的所有scoket。

3. 流的数据是单向的，而scoketChannl是双向的，既可以向通道中写数据，也可以从通道中读数据，并且通道中的数据读写都是通过buffer实现的。


















