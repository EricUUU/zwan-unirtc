# zwan-rtc 实时音视频组件SDK接口文档

## 简介：

zwan-rtc sdk组件 由紫万科技提供实时音视频原生插件，支持单人或多人实时是音视频，支持文本消息传输，支持海康摄像头直播视频流 等等功能，适用于教育，政府，医疗跨地区的视频会议系统，支持快速的私有化部署。

### 组件引用说明

###### 1.在package.json文件中 引用原生component插件   并添加插件aar包到程序中

```vue
 "plugins": [ 
			{
				"type": "component",
				"name": "myVideo",
				"class": "com.zwan.unizwanrtc.ZwanVideoView"
			}
		]
```



###### 2.创建nvue页面 将视频显示组件布局到页面中

```vue
<myVideo ref="myVideo" @onMsg="onMsg" @onQueryDevice="onQueryDevice" @onQos="onQos" @onQuery="onQuery" style="width:100%;height:500px"></myVideo>
```



###### 3.当页面渲染完成后 初始化插件SDK

@parameter STUN服务器地址:stun:stun.zwan.com.cn:2231

@parameter WebSocket服务器地址:ws://183.66.37.186:2338

```vue
this.$refs.myVideo.ZwanSDK_Init("stun:stun.zwan.com.cn:2231", "ws://183.66.37.186:2338")
```



###### 4.初始化设置 SDK回调

```vue
this.$refs.myVideo.ZwanSDK_InitConfiguration()
```



###### 5.SDK 接口 API说明

```vue
this.$refs.telVideo.ZwanSDK_Login("UniAPP", "UniAPP房间")  登录 （用户名，房间名）
```

```vue
ZwanSDK_StreamStart（int type,int mode, int picmode, int picnum, int mcuwidth, int mcuheight, int mcufps, int mcuminbr, int mcuinitbr, int mcumaxbr, boolean sendaudio, boolean recvaudio, boolean sendvideo, boolean recvvideo, boolean publishaudio, boolean publishvideo, boolean subscribeaudio, boolean subscribevideo, String transport, String videocodec）视频推流

this.$refs.myVideo.ZwanSDK_StreamStart(2, 2, 0, 1, 1920, 1080, 15, 300000, 800000, 1500000, true, true,true, true, true, true, true, true, "udp", "H264") //参考参数调用

ZwanSDK_StreamStop（）停止推流

ZwanSDK_Logout（）登出

ZwanSDK_SendMsg(String msg,String receiver) 发送消息（消息，发送人）

ZwanSDK_MicMute() 麦克风开关

ZwanSDK_SpeakerMute() 扬声器开关

ZwanSDK_SwitchAroundCamera() 前后置切换

ZwanSDK_SwitchScreen() 切换到共享屏幕

ZwanSDK_SwitchCamera() 切换到摄像头

ZwanSDK_Querycall(String room) 查询会议成员(房间名)

ZwanSDK_Querydevice() 查询海康摄像头列表

ZwanSDK_StreamStartDevice(String account,int mode, int picmode, int picnum, int mcuwidth, int mcuheight, int mcufps, int mcuminbr, int mcuinitbr, int mcumaxbr, boolean sendaudio, boolean recvaudio, boolean sendvideo, boolean recvvideo, boolean publishaudio, boolean publishvideo, boolean subscribeaudio, boolean subscribevideo, String transport, String videocodec) 远端海康摄像头推流

ZwanSDK_StreamStopDevice(String account)  远端海康摄像头停止推流（远端账户）
```



###### 6.视频推流详细参数说明

```vue
"type"：1:摄像头1 2:摄像头2 3：屏幕共享

"mode" : 0,     //会议模式： 0：统一编码MCU会议 1：独立编码MCU会议 2：统一控制SFU会议 3：独立控制SFU会议

  "picmode" : 0,    //画面模式： 0：一大多小， 1：等大

  "picnum" : 9,    //最大显示画面数： 1：将启动SFU会议， 4： 将启动最大4分屏MCU会议 9：将启动最大9分屏MCU会议

  "mcuwidth": 1280,  //如果是MCU会议，则会场的分辨率宽度，建议为1280

  "mcuheight": 720,  //如果是MCU会议，则会场的分辨率高度，建议为720

  "mcufps": 15,    //如果是MCU会议，则会场的帧率，建议设置为15

  "mcuminbr": 300000, //如果是MCU会议，本端最小的接收码率，用于独立编码MCU会议

  "mcuinitbr": 800000, //如果是MCU会议，本端初始接收码率，独立编码MCU会议做初始码率用。 如果是统一编码MCU会议，则服务端以该码率统一编码

  "mcumaxbr": 1500000, //如果是MCU会议，本端最大的接收码率，用于独立编码MCU会议

  "sendaudio": true, //本终端是否发送音频

  "recvaudio": true, //本终端是否接收音频

  "sendvideo": true, //本终端是否发送视频

  "recvvideo": true, //本终端是否接收视频

  "publishaudio": true, //本终端音频是否入会发布，前提是sendaudio为true

  "publishvideo": true, //本终端视频是否入会发布，前提是sendvideo为true

  "subscribeaudio": true, //本终端是否入会收听会场音频，前提是recvaudio为true

  "subscribevideo": true, //本终端是否入会收看会场视频，前提是recvvideo为true

  "transport": "udp",   //本终端传输媒体流是采用udp还是tcp，udp：服务器将只下发udp候选端口，tcp：服务器将只下发tcp候选端口

"videocodec": "vp8"   //可选参数，设置会议室编码器类型，默认未vp8,可设置为"h264"
```



###### 7.SDK回调侦听接收

```vue
<myVideo ref="myVideo" @onMsg="onMsg" @onQueryDevice="onQueryDevice" @onQos="onQos" @onQuery="onQuery" style="width:100%;height:500px"></myVideo>
```

​			

```vue
onMsg: function(e) {
		console.log("消息:" + e.detail.message);
},
onQueryDevice: function(e) {
​		console.log("查询设备:" + e.detail.querydevice);
},
onQos: function(e) {
​		console.log("QOS信息:" + e.detail.qos);
},
onQuery: function(e) {
​		console.log("Query信息:" + e.detail.querycall);
}


```

###### 8.关于

$$
若有疑问，请联系QQ：914814914
$$

