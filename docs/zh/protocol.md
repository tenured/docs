# 通信协议说明

通信协议内容

| 字段    | 类型   | 说明                                                         |
| :------ | ------ | ------------------------------------------------------------ |
| id      | uint32 | 消息ID，每个消息均要发送且每次发送都要递增，这样用户区分用户发送的消息 |
| code    | uint16 | 消息类型，查阅RequestCode，如果（flag & 0b10 > 0）&& code != 0，返回请求错误值ResponseCode。 |
| version | uint8  | 当前消息版本号，用户兼容相同消息的不同的版本(预留了255个可升级) |
| flag    | uint2  | 第一位标识是否是请求，0=请求，1=ACK。第二位：                |
| header  | byte[] | 消息内容header，此处内容用户出消息头的传递。可以为空         |
| body    | byte[] | 消息内容body，用户传递消息内容体字节流。可用为空             |


## TCP通信协议

[![RemotingCommand](https://github.com/ihaiker/tenured/blob/master/docs/RemotingCommand.png)](https://www.processon.com/view/link/5c6e6964e4b08a7683c9bee7)


## WebSocket通信协议

采用JSON解析上诉通信协议内容。

`注意:body采用base64进行编码，header是json格式的字符串`

例如：

```json
#json内容
{
    "id":11,"code":1,"version":1,"flag":0,
    "header":"{'filed':1}","body":"YmFzZTY0"
}
# 解析结果
header = {'filed':1}
body = basse64
```



## RequestCode

```shell
REQUEST_CODE_AUTH=1 #用户登录认证消息
REQUEST_CODE_PEER_MSG=2 #用户发送点对点消息
REQUEST_CODE_GROUP_MSG=3 #用户发送群组（聊天室）消息
```

## ResponseCode

```shell
RESPONSE_CODE_SUCCESS=0
RESPONSE_CODE_ERROR_NO_AUTH=1 #客户端连接未提供认证信息
RESPONSE_CODE_ERROR_NOT_A_FIREND=2 #点对点消息不能发送，对方开启好友认证，且对方并非好友。
RESPONSE_CODE_ERROR_NOT_A_GROUP_MEMBER=3 #发送群组消息，并且用户不在群组中
```