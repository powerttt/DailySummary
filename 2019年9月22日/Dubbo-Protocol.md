# Dubbo远程调用

## Dubbo调用介绍

简单的RPC调用，需要把服务调用信息传递到服务端，每次调用的一些公用信息包括服务调用接口、方法名、方法参数类型和参数值等，在传递方法参数数值时，需要先序列化对象并经过网络传输到服务端，在服务端按照客户端序列化顺序在一次反序列化来读取信息，然后拼装成请求对象进行服务反射调用，最终将调用结果传给客户端。



##### Dubbo调用实现原理

首先在客户端启动时在注册中心拉取和订阅对应的服务列表，Cluster会把拉取的服务列表聚合成一个Invoker，每次RPC调用会通过Directory#list获取provider地址（已经生成的Invoker列表），获取服务列表后进行路由和负载均衡。之后将请求交给底层I/O线程池（比如Netty）处理，I/O线程池主要处理读写，序列化和反序列化等逻辑，因此在这里一定不能阻塞操作。



## Dubbo协议详解

Dubbo协议参考了TCP/IP协议，一次RPC调用包括**协议头**和**协议体**两部分。

16字节长的报文头部主要携带了**魔法数（0xdabb）**、**Request or Response**、**需要往返**、**心跳**、**事件**、**报文内序列化协议编号**、**请求体length**、**请求唯一标识**。



请求消息体：**Dubbo Version** 、**服务接口名**、**Version**、方法名、参数类型、方法参数值、请求额外参数。

##### 请求对象编码 ExcangeCodec#encodeRequest

```java
protected void encodeRequest(Channel channel, ChannelBuffer buffer, Request req) throws IOException {
        // 获取指定或默认的序列化协议（Hessian2）
        Serialization serialization = getSerialization(channel);
        // header.----构造16字节头
        byte[] header = new byte[HEADER_LENGTH];
        // set magic number.--->占用两个字节存储魔法数
        Bytes.short2bytes(MAGIC, header);

        // set request and serialization flag.
        // 在第3个字节（16位和19~23位）分别存储请求标志和序列化协议序号(HESSIAN2_SERIALIZATION_ID = 2;)
        header[2] = (byte) (FLAG_REQUEST | serialization.getContentTypeId());
        // isTwoWay是否位双向，设置请求/响应标记
        if (req.isTwoWay()) {
            header[2] |= FLAG_TWOWAY;
        }
        if (req.isEvent()) {
            header[2] |= FLAG_EVENT;
        }

        // set request id.请求唯一标识
        Bytes.long2bytes(req.getId(), header, 4);

        // encode request data.
        int savedWriteIndex = buffer.writerIndex();
        // 跳过buffer头部16个字节，用于序列化消息体
        buffer.writerIndex(savedWriteIndex + HEADER_LENGTH);
        ChannelBufferOutputStream bos = new ChannelBufferOutputStream(buffer);
        ObjectOutput out = serialization.serialize(channel.getUrl(), bos);
        if (req.isEvent()) {
            encodeEventData(channel, out, req.getData());
        } else {
            encodeRequestData(channel, out, req.getData(), req.getVersion());
        }
        out.flushBuffer();
        if (out instanceof Cleanable) {
            ((Cleanable) out).cleanup();
        }
        bos.flush();
        bos.close();
        int len = bos.writtenBytes();
        // 检查文件是否超过8MB
        checkPayload(channel, len);
        // 向消息长度写入头部第12个字节的偏移量（96~127位）
        Bytes.int2bytes(len, header, 12);

        // write
        buffer.writerIndex(savedWriteIndex);
        buffer.writeBytes(header); // write header.
        buffer.writerIndex(savedWriteIndex + HEADER_LENGTH + len);
    }
```



##### 编码请求对象体 DubboCodec#encodeRequestData

```java

```







