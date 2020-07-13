![image](https://images.unsplash.com/photo-1594293185787-c6705bb6eccf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&q=60)
### vue + webSocket 实时任务信息通知

##### [websocket](http://www.ruanyifeng.com/blog/2017/05/websocket.html)
WebSocket 协议在2008年诞生，2011年成为国际标准。所有浏览器都已经支持了。
它的最大特点就是，服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。
> 特点  
> - 建立在TCP协议之上，服务端的实现比较容易；
> - 与HTTP协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用HTTP协议，因此握手时不容易屏蔽，能通过各种HTTP代理服务器；
> - 数据格式比较轻量，性能开销小，通信高效；
> - 可以发送文本，也可以发送二进制数据
> - 没有同源限制，客户端可以与任意服务器通信
> - 协议标识符是 **WS**(如果加密，则为**WSS**)，服务器网址就是URL  

![image](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017051503.jpg)

##### VUE + WebSocket 长链接实现
在项目的创建 utils/websocket.js

```
<!--引入store，用于管理socket推送来的消息-->
import store from '../store'
<!--封装websocket对象-->
const WS = {
    $ws:null, // webscoket实例
    wsUrl: 'ws://xxxxx.com:80/xxx', // websocket链接地址
    <!--初始化webSocket-->
    createWS：function(){
        if('WebSocket' in window){
            this.$ws = new WebSocket(wsURl)
            this.$ws.onopen = this.wsOpen
            this.$ws.onmessage = this.wsMessage
            this.$ws.onerror = this.wsError
            this.$ws.onclose = this.wsClose
        } else {
            alert('当前浏览器不支持webSocket')
        }
    },
    <!--webSocket 打开-->
    wsOpen: function() {
        this.$ws.send('连接成功')
        console.log('== websocket open ==')
        <!--开始心跳-->
        heartBeat.start()
    },
    <!--websocket 接收到服务器消息-->
    wsMessage:function(msg) {
        console.log('== websocket message ==', msg)
        <!--接受到消息，重置心跳-->
        heartBeat.reset()
        store.commit('SET_WS_MSG', msg.data)
    },
    <!--websocket 发生错误-->
    wsError: function(err){
        console.log('== websocket error ==', err)
    },
    <!--websocket 关闭连接-->
    wsClose: function(event){
        console.log('== websocket close ==', event)
    }
}
<!--webSocket 心跳-->
const heartBeat = {
    timeout:30000, // 心跳重连时间
    timeoutObj:null, // 定时器
    reset:function(){
        clearInterVal(this.timeoutObj)
        console.log('websocket 心跳')
        WS.start()
    },
    start:function(){
        this.timeoutObj = setTimeout(function(){
            if(WS.$ws.readyState === 1){
                WS.$ws.send('HeartBeat')
            }
        },this.timeout)
    }
}
export default WS
```
在main.js中引入WS，挂载到Vue原型上

```
    import Vue from 'vue'
    import WS from '@/util/websocket'
    Vue.prototype.$ws = WS
```

在store/index.js创建全局数据存储

```
    const store= new Vuex.Store({
        state:{
            webSocketMsg:''
        },
        mutations:{
            SET_WS_MSG (state, msg) =>{
                state.webSocketMsg = msg
            }
        }
    })
```
在单个组件内部使用
```
    computed:{
        getWsMsg (){
            return this.$store.state.webSocketMsg
        }
    },
    watch:{
        getWsMsg:{
            handler: function(newVal) {
                console.log(newVal)
                alert('接收到webSocket推送'+ newVal)
            }
        }
    }
```
如果要在所有的界面都能接收socket推送消息，并弹出提示可以在App.vue中监听

```
    computed:{
        getWsMsg (){
            return this.$store.state.webSocketMsg
        }
    },
    watch:{
        getWsMsg:{
            handler: function(newVal) {
                console.log(newVal)
                alert('接收到webSocket推送'+ newVal)
            }
        }
    }

```
参考文档
- [WebSocket 教程](http://www.ruanyifeng.com/blog/2017/05/websocket.html)
- [WebSocket加入心跳包防止自动断开连接](https://www.jianshu.com/p/1141dcf6de3e)