# Realtime Chatting

Message Service: manage msg, bootstrap, state persistence

Realtime Service: realtime push

Channel Service: group chat on behalf and may also persist online status

MsgSvc storage:

点开微信看我有哪些thread,

Thread table sharded by user id, but need second index thread id for real time service SQL

MsgTable row key thread id , col key timestamp

Solution: send msg, write to msg Table

Receive msg: pull per 10s.

![](/assets/chatMsgTbl.png)加速

![](/assets/rt.png)

群聊

![](/assets/groupchat.png)Rate Limiter, datadog, 力度

