# Pubsub

> Pubsub库是一款基于订阅发布消息的实现组件通信的库。

## 用法

```javascript
let token = PubSub.subscribe('add', (msg, data)=>{ console.log('aaa')});
PubSub.subscribe('add', (msg, data)=>{ console.log('aaa')}); // 订阅消息时，消息名称可以同名。
PubSub.publish('add','数据'); // 发布方法，第二个参数为通信数据，如果需要传多个数据，可以封装成对象
console.log('查看publish是不是异步方法');

PubSub.publishSync('add', '数据'); // publishSync是同步方法
console.log('查看publishSync是不是异步方法');

PubSub.unsubscribe(token); 根据token值，删除订阅的对应token的消息订阅。
PubSub.unsubscribe('add'); 根据消息名，删除订阅的所有同名消息
```

