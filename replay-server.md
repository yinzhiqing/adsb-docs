# replay server

  回放服务用于回放存储的ADSB数据，数据源为各站存储的历史数据。 服务通过websocket协议与客户端通信，客户端通过json格式下发指令与服务端通信。

#  功能：
    - 支持多用户连接
    - 支持回放速度控制
    - 支持回放数据的时间范围选择
    - 支持回放数据的位置调整

#  使用方法：
    - 选择数据源
    - 选择时间范围
    - 选择回放速度
    - 点击开始回放按钮
    - 点击暂停按钮
    - 点击停止按钮
    - 点击退出按钮


#  指令列表


| 指令 | 说明 | 参数 | 示例 |
| --- | --- | --- | --- |
| start | 开始回放 | start stop speed station| {"type":"start", "start": 0, "stop": 1722233089958, "speed": [-]2, "station": "hx"} |
| pause | 暂停回放 | 无 | {"type":"pause"} |
|recover | 恢复回放 | 无 | {"type":"recover"} |
| stop | 停止回放 | 无 | {"type":"stop"} |
| speed | 设置回放速度 | speed | {"type":"speed","speed":2} |

# 返回值

  返回值为json格式，包括命令结果和数据内容。

# 数据格式
## 飞机数据示例

| 字段 | 说明 | 示例 |
| --- | --- | --- |
| class| 数据大类| 固定值：ADSBDATA |
| meta | 数据小类 | 飞机数据：ADSBAIRCRAFT; 命令数据： ADSBCMD |


-数据内容示例-：
```
{
    "class": "ADSBDATA",
    "meta": "ADSBAIRCRAFT",
    "icao": 3958541,
    "flight": "",
    "latitude": 39.942214965820312,
    "longitude": 116.7530517578125,
    "altitude": 7600,
    "speed": 0,
    "speedType": 0,
    "heading": 0,
    "verticalRate": 0,
    "emitterCategory": "Large",
    "status": "No emergency",
    "squawk": 0,
    "range": 0,
    "azimuth": 0,
    "elevation": 0,
    "time": 1722233089958,
    "positionValid": false,
    "altitudeValid": true,
    "speedValid": false,
    "headingValid": false,
    "verticalRateValid": false,
    "adsbFrameCount": 2,
    "minCorrelation": 7.40802526473999,
    "maxCorrelation": 7.40802526473999,
    "correlation": 7.40802526473999,
    "RSSI": 1.4532318459714588e-07,
    "adsbVersion": -1,
    "NACp": -1,
    "subType": -1,
    "nicS": -1,
    "nicA": -1,
    "nicBC": 0,
    "NIC": -1
}
```


## 命令类型示例


| 字段 | 说明 | 示例 |
| --- | --- | --- |
| class| 数据大类| 固定值：ADSBDATA |
| meta | 数据小类 | 飞机数据：ADSBAIRCRAFT; 命令数据： ADSBCMD |
| status | 命令执行状态 | ok: 成功; fail: 失败 |
| data | 命令内容 | {"type":"start", "start": 0, "stop": 1722233089958, "speed": [-]2, "station": "hx"} |

命令类型示例

```
{
    "class": "ADSBDATA",
    "meta": "ADSBCMD",
    "status": "ok",
    "cmd": {
        "type": "stop",
        "start": 1,
        "stop": 1723915691311,
        "station": "hx",
        "speed": 1
    }
}
```

# 指令转换有效性

- start: 仅在停止状态下有效
- pause: 仅在运行状态下有效     [start | recover | speed] -> pause
- recover: 仅在暂停状态下有效   [pause] -> recover
- stop: 仅在运行状态下有效      [start | recover | pause] -> stop
- speed: 仅在运行状态下有效     [start | recover | restart] -> speed
- restart : 仅在停止状态下有效  [stop | pause | start | recover] -> restart
    
