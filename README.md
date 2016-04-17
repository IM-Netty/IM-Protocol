# IM-Protocol
使用Netty 实现的IM 系统-协议文档

> 实现一个IM 系统，需要自定义一个协议，协议包括协议头(header)和协议体(body)，模仿TeamTalk 的通信协议我们也定义协议

## 协议头

字段 | length | service_id | command_id | version | reserved
---- | ------ | ---------- | ---------- | ------- | -------- 
占字节数 | 4 | 2 | 2 | 2 | 2
说明 | 二进制数据包长度<br >length=header.length + body.length | 服务号 | 命令号 | 协议版本号 | 保留，可用于系列号等

具体的响应数据包中的module_id，reserved于发送过来的数据包一致，version暂时恒等于1，command_id由具体的请求/响应的定义指定。

## 协议体

> 一帧数据包括协议头定义，根据不同服务号和命令号定义不同的"数据格式"，"数据格式"按照数据包里排列的顺序依次排开。

###注：

>1. Req：指从外部发到BusinessServer的请求数据包
>2. Resp：指BusinessServer收到Req之后业务处理的响应数据包
>3. Out: 系统主动发出的请求
>4. In: 系统发出请求外部给的响应
>5. Notify: 状态通知，客户端发送给文件服务器或文件服务器通知客户端，无需回应
>6. 数据格式: 字段顺序按照列出的顺序排列

类型 | command_id | 数据包说明 | 数据格式<br >(名称:字节数,数据项说明) | 消息返回的command_id | 备注
---- | ------ | ---------- | ---------- | ------- | -------- 
Req | 1 | 登录 |  | 2 | service_id：1
Rsp | 2 | 登录响应 | result：4，结果 | | service_id：1 <br > result:0,成功