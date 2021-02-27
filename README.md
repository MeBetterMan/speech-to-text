# speech-to-text
    本项目使用技术:本项目前端使用vue+websocket，后端使用mina拉取tcp数据，然后通过WebSocketClient订阅ws语音引擎地址,传递tcp拉取的数据进行翻译。返回的翻译数据通过一个队列缓存。WebsocketServer实现前端订阅，然后将所有连接到WebSocketServer的前端连接缓存到一个list.  
    开启一个新的线程，负责将队列中的数据发送给所有的连接到WebSocketServer的client。
