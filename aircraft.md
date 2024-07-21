// 从 ADS-B 帧中提取的关于飞机的数据
-   class            ADSBDATA
-   meta             ADSBAIRCRAFT 
-   icao24位         ICAO   飞机地址，唯一标识飞机
-   flight           飞行呼号，代表航班号的字母数字字符串
-   latitude          纬度，飞机的位置
-   longitude         经度，飞机的位置
-   altitude          高度，以英尺表示，海平面以上的高度
-   speed             速度，以节表示，飞机的速度
-   speedType         枚举，指示速度的类型（地速、空速等）
-   heading           航向，以度表示，飞机面对的方向
-   verticalRate      垂直爬升率，以英尺/分钟表示，高度变化率
-   emitterCategory   飞机类型，根据大小和角色对飞机进行分类
-   status            飞机状态，关于飞行状态的额外信息
-   squawk            Mode-A 编码，用于空中交通管制(ATC)的四位代码
-   range             从站点到飞机的距离，以海里表示
-   azimuth           从站点到飞机的方位角，以度表示
-   elevation         从站点到飞机的仰角，以度表示
-   time;             最后更新时间，最后更新的UNIX时间戳
-   positionValid     表示纬度和经度数据是否有效
-   altitudeValid     表示高度数据是否有效
-   speedValid        表示速度数据是否有效
-   headingValid      表示航向数据是否有效
-   verticalRateValid 表示垂直爬升率数据是否有效
-   // 使用两个 CPR 帧计算位置的状态
-   cprValid          标志，表示CPR数据是否有效以进行计算
-   cprLat            在计算位置时的纬度临时存储
-   cprLong           在计算位置时的经度临时存储
-   cprTime           CPR帧的时间戳
-   adsbFrameCount    此飞机接收到的 ADS-B 帧数量
-   minCorrelation    最小相关性值，用于信号质量分析
-   maxCorrelation    最大相关性值，用于信号质量分析
-   correlation       当前相关性值，用于信号质量分析
-   correlationMau    相关性的移动平均工具，平滑数据
-   RSSI              接收信号强度指示器，测量信号功率
-   rssiMau           RSSI的移动平均工具，平滑信号强度数据
-   adsbVersion       ADS-B 版本，表示使用的 ADS-B 协议的版本
-   NACp              位置的导航精度类别，表示位置数据的精度
-   subType           ADS-B 消息的子类型，提供关于消息类型的额外信息
-   nicS              导航完整性类别，补充，提供额外的完整性信息
-   nicA              导航完整性类别，A 类型，表示位置数据的完整性
-   nicBC             导航完整性类别，B 和 C 类型，进一步的完整性信息
-   NIC               导航完整性类别，导航数据的总体完整性级别


```
{
    "class": "ADSBDATA",     //数据种类
    "meta": "ADSBAIRCRAFT",  //数据类型
    "icao": 7864956,
    "flight": "",
    "latitude": 0,
    "longitude": 0,
    "altitude": 0,
    "speed": 0,
    "speedType": 0,
    "heading": 0,
    "verticalRate": 0,
    "squawk": 0,
    "range": 0,
    "azimuth": 0,
    "elevation": 0,
    "time": 1721384294,
    "positionValid": false,
    "altitudeValid": false,
    "speedValid": false,
    "headingValid": false,
    "verticalRateValid": false,
    "adsbFrameCount": 2,
    "minCorrelation": 24.260040283203125,
    "maxCorrelation": 24.260040283203125,
    "correlation": 24.260040283203125,
    "RSSI": 3.1513701514995773e-07,
    "adsbVersion": -1,
    "NACp": -1,
    "subType": -1,
    "nicS": -1,
    "nicA": -1,
    "nicBC": -1,
    "NIC": -1
}
```

```
"class": "ADSBDATA",
    "meta": "ADSBAIRCRAFT",
    "icao": 7871915,
    "flight": "",
    "latitude": 39.826696363546077,
    "longitude": 116.03759765625,
    "altitude": 19700,
    "speed": 437,
    "speedType": 0,
    "heading": 158,
    "verticalRate": 0,
    "status": "No emergency",
    "squawk": 3003,
    "range": 0,
    "azimuth": 0,
    "elevation": 0,
    "time": 1721384298,
    "positionValid": false,
    "altitudeValid": true,
    "speedValid": true,
    "headingValid": true,
    "verticalRateValid": true,
    "adsbFrameCount": 9,
    "minCorrelation": 11.617575645446777,
    "maxCorrelation": 118.66923522949219,
    "correlation": 47.43719482421875,
    "RSSI": 1.043022848534747e-06,
    "adsbVersion": 2,
    "NACp": 9,
    "subType": 0,
    "nicS": -1,
    "nicA": 0,
    "nicBC": 0,
    "NIC": 8
}
```
