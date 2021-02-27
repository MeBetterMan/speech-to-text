<template>
  <div class="tab-control">
    <h2 id="header">语音识别转换示例程序</h2>
    <p id="nav1">{{ final_msg }}{{ msg }}</p>
  </div>
</template>

<script>

export default {
    data () {

        return {
            path: "ws://"+window.location.hostname+":8887",
            socket:"",
            msg:"",
            final_msg:""    
        }                             
    },
    mounted () {
        // 初始化
        this.init()
    },
    methods: {
        init: function () {
            if(typeof(WebSocket) === "undefined"){
                alert("您的浏览器不支持socket")
            }else{
                // 实例化socket
                this.socket = new WebSocket(this.path)
                // 监听socket连接
                this.socket.onopen = this.open
                // 监听socket错误信息
                this.socket.onerror = this.error
                // 监听socket消息
                this.socket.onmessage = this.getMessage
            }
        },
        open: function () {
            console.log("socket连接成功")
        },
        error: function () {
            console.log("连接错误")
        },
        getMessage: function (msg) {
            var rsp
            var msgData = msg.data
            if(msgData.indexOf("Wel") != -1 || msgData === null || msgData === undefined || msgData === ''){
                rsp=""
            }else{
               rsp = JSON.parse( msg.data)
               var hypothesesArr = rsp.result.hypotheses
               var final = rsp.result.final
               var h
               console.log(hypothesesArr)
               var finalWords
               var word = ""
               var final_word
               hypothesesArr.forEach(element => {
                    word = word + element.transcript
                   if(final){
                       this.final_msg = this.final_msg + "\n"+word
                       this.word = ""
                       console.log(final)
                   }else{
                       this.msg = word
                   }
               });
            }
        },
        send: function () {
            this.socket.send("test from brower")
        },
        close: function (e) {
            console.log("socket已经关闭"+"错误原因" + e.reason)
             // 实例化socket
                this.socket = new WebSocket(this.path)
                // 监听socket连接
                this.socket.onopen = this.open
                // 监听socket错误信息
                this.socket.onerror = this.error
                // 监听socket消息
                this.socket.onmessage = this.getMessage
        },
        destroyed () {
        // 销毁监听
        this.socket.onclose = this.close
    },


    getIP:function ()  {
        var os = require('os')
        const interfaces = os.networkInterfaces();
        console.log('interfaces:', interfaces)
        for (let devName in interfaces) {
            const iface = interfaces[devName];
            console.log('iface:', iface)
            for (let i = 0; i < iface.length; i++) {
                const alias = iface[i];
                console.log('alias:', alias)
                if (alias.family === 'IPv4' && alias.address !== '127.0.0.1' && !alias.internal && alias.netmask === '255.255.255.0') {
                    return alias.address;
                }
            }
        }
    }
    
    }
}
</script>

<style>
.text-wrapper {
  white-space: pre-wrap;
}
body {
  width: 100%;
  height: 100%;
  position: relative;
  background-color: d0d0d0;
}

#header {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  color: rgb(46, 41, 41);
  text-align: center;
  padding: 5px;
  margin: 2% 20%;
  font-size: 200%;
}
#nav {
  color: rgb(46, 44, 44);
  text-align: left;
  font-size: 150%;
  line-height: 30px;
  background-color: #eeeeee;
  height: 500px;
  width: 1500px;
  float: center;
  padding: 5px;
  margin: auto auto;
  border: 1px solid #000;
  overflow: auto;
}
#section {
  color: rgb(44, 42, 42);
  text-align: left;
  font-size: 150%;
  float: center;
  margin: 0 auto;
  border: 1px solid #000;
  height: auto;
  width: 1500px;
}
#nav1 {
  font-size: 150%;
  font-family: "Times New Roman", Times, serif;
  color: white;
  overflow-y: scroll;
  display: block;
  position: fixed;
  _position: absolute;
  top: 40%;
  left: 30%;
  width: 1500px;
  height: 600px;
  margin-left: -333px;
  margin-top: -200px;
  z-index: 10001;
  box-shadow: 2px 2px 4px #a0a0a0, -2px -2px 4px #a0a0a0;
  background-color: #2c3e50;
}
</style>