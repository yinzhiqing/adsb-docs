# heatmap server
    热力图服务用于展示各站的热力图数据，数据源为各站存储的历史数据。 服务通过websocket协议与客户端通信，客户端通过json格式下发指令在服务端实时生成热力图，并以二进制格式从服务端获取。
    
    默认端口：8820

## 功能：
    - 支持多用户连接
    - 支持热力图数据的时间范围选择
    - 支持热力图数据的位置调整
    - 支持热力图数据的颜色调整
    - 支持滚动生成热力图
    - 支持滚动生成过程中修改参数（时间点继续向后推进)

## 使用方法：
    - 选择热力图参数(时间范围，颜色，位置等)
    - 点击开始生成按钮
    - 点击停止按钮

## 指令列表
    - start: 开始生成热力图
    - stop: 停止生成热力图
    - update: 更新热力图参数

## 返回值
   二进制数据流，包括热力图数据内容。 

## 通信协议

### 客户端发送数据格式
   客户端以json格式发送数据到服务端，数据格式(持续更新中)如下：

```json
{
   "type":"start",                   // 指令类型: start: 开始生成热力图, stop: 停止生成热力图, update: 更新热力图参数
   "start": 0,                       // unix timestamp, 毫秒, 过滤数据条件，开始时间
   "stop": 1725369279462,            // unix timestamp, 毫秒, 过滤数据条件，结束时间
   "station":"hx"                    // 站点名称
   "replay": {                       // 重放参数, 可选, 默认不重放, 重放模式为一次性模式时候， interval和step无效, 可不设置。
        interval: 8,                    // 重放间隔，单位秒
        step: 900,                      // 重放数据步长, 单位秒
        mode: ""                        // 累积模式(acc)， 分段模式（seg)， 一次性模式(dis)， 默认为一次性模式
   }
   "thresholds": {                   // 过滤条件, 可选, 默认NIC = 7, NACP = 0
        "nic": 7,                       // nic阈值, =0时不作为过滤条件， >0时，只显示nic小于该值的数据
        "nacp": 0                       // nacp阈值, =0时不作为过滤条件， >0时，只显示nacp小于该值的数据
   },
   "info": {                         // 热力图参数
        "resolution": {                 // 热力图分辨率, 可选, 默认为400*400
            "width": 400,                   // 宽度
            "height": 400                   // 高度
        },
        "radius": 10,                   // 热力图半径, 单位像素, 可选, 默认为10
        "color": {                      // 热力图颜色, 可选, 默认为红色
            "name": "orrd",                 // 颜色名称: 可选， 默认为红色
            "variant": "discrete"           // 颜色变种: 可选， 默认为离散
        }
   },
   "region": {                      // 热力图显示区域 
        "left_top": {                   // 热力图显示区域左上角坐标
            "longitude":115.123,            // 经度
            "latitude":35.5324              // 纬度
        },
        "right_bottom": {               // 热力图显示区域右下角坐标
            "longitude": 117.13445,     // 经度
            "latitude":45.343           // 纬度
        }
   }
}

```

### 示例
   以下是常用示例说明，持续更新中。 
   
#### 示例1

   根据指定条件生成热力图，条件如下：
   
    - 1. 生成模式： 一次性
    - 2. 过滤nic<7的数据
    - 3. 指定图片分辨率为800*800
    - 4. 热力图半径用默认值
    - 5. 采用默认颜色
    - 6. 显示区域为左上角(115.123, 35.5324)和右下角(117.13445, 45.343)的区域
    - 7. 开始时间为0， 结束时间为UTC 1725369279462(毫秒）

```json
{
   "type":"start",
   "start": 0,
   "stop": 1725369279462,
   "station":"hx",
   "replay": {
        "mode": "dis"
   },
   "thresholds": {
        "nic": 7,
   },
   "info": {
        "resolution": {
            "width": 800,
            "height": 800
        },
   },
   "region": {
        "left_top": {
            "longitude":115.123,
            "latitude":35.5324
        },
        "right_bottom": {
            "longitude": 117.13445,
            "latitude":45.343
        }
   }
}
```
   
#### 示例2

   根据指定条件生成热力图，条件如下：
   
     - 1. 生成模式： 分段模式， 间隔7秒， 步长900秒
     - 2. 过滤nic<7的数据
     - 3. 指定图片分辨率为900*900
     - 4. 热力图半径为5
     - 5. 采用默认颜色
     - 6. 显示区域为左上角(115.123, 35.5324)和右下角(117.13445, 45.343)的区域
     - 7. 开始时间为0， 结束时间为UTC 1725369279462(毫秒）

```json
{
   "type":"start",
   "start": 0,
   "stop": 1725369279462,
   "station":"hx",
   "replay": {
        "interval": 7,
        "step": 900,
        "mode": "seg"
   },
   "thresholds": {
        "nic": 7,
   },
   "info": {
        "resolution": {
            "width": 900,
            "height": 900
        },
        "radius": 5
   },
   "region": {
        "left_top": {
            "longitude":115.123,
            "latitude":35.5324
        },
        "right_bottom": {
            "longitude": 117.13445,
            "latitude":45.343
        }
   }
}
```


#### 示例3

   根据指定条件生成热力图，条件如下：
   
     - 1. 生成模式： 累积模式， 间隔12秒， 步长900秒
     - 2. 过滤nic<7的数据
     - 3. 指定图片分辨率为1100*700
     - 4. 热力图半径为5
     - 5. 采用blues颜色, variant = soft
     - 6. 显示区域为左上角(115.123, 35.5324)和右下角(117.13445, 45.343)的区域
     - 7. 开始时间为0， 结束时间为UTC 1725369279462(毫秒）

```json
{
   "type":"start",
   "start": 0,
   "stop": 1725369279462,
   "station":"hx",
   "replay": {
        "interval": 12,
        "step": 900,
        "mode": "acc"
   },
   "thresholds": {
        "nic": 7,
   },
   "info": {
        "resolution": {
            "width": 1100,
            "height": 700
        },
        "radius": 5,
        "color": {
            "name": "blues",
            "variant": "soft"
        }
   },
   "region": {
        "left_top": {
            "longitude":115.123,
            "latitude":35.5324
        },
        "right_bottom": {
            "longitude": 117.13445,
            "latitude":45.343
        }
   }
}

```


### 服务端返回数据格式

服务端返回的数据是二进制格式， 包括热力图部分及热力图数据两部分内容。 以下是不同语言解析方法。

```javascript
    var head = new DataView(data);

    /* frame head */
    var frameLeader = head.getUint32(0, true);
    var frameVer    = head.getUint16(4, true);
    var frameSTC    = head.getUint32(6, true);
    var frameTSyear = head.getUint16(10, true);
    var frameTSmon  = head.getUint8(12, true);
    var frameTSday  = head.getUint8(13, true);
    var frameTShour = head.getUint8(14, true);
    var frameTSmin  = head.getUint8(15, true);
    var frameTSsec  = head.getUint8(16, true);
    var frameTSmsec = head.getUint16(17, true);
    var framePL     = head.getUint32(19, true);
    var frameEL     = head.getUint8(23, true);
    
    /** payload head */
    var payloadType      = head.getUint8(24 + frameEL, true);
    var payloadLength    = head.getUint32(25+ frameEL, true);
    
    /** image head payloadType == 36 */
    var imageWidth       = head.getInt32(29 + frameEL, true);
    var imageHeight      = head.getInt32(33 + frameEL, true);
    var imageBytes       = head.getInt32(37 + frameEL, true);
    var imageNorth       = head.getFloat64(41 + frameEL, true);
    var imageSouth       = head.getFloat64(49 + frameEL, true);
    var imageEast        = head.getFloat64(57 + frameEL, true);
    var imageWest        = head.getFloat64(65 + frameEL, true);
    var imageAltitude    = head.getFloat64(73 + frameEL, true);
    
    var image = {
        type: payloadType,
        head: {
            width: imageWidth,
            height: imageHeight,
            bytes: imageBytes,
            north: imageNorth,
            south: imageSouth,
            east: imageEast,
            west: imageWest
        },
        data: new Uint8Array(data, 81 + frameEL)
    };
    
    /* create image url */
    var blob = new Blob([image.data], { type: "image/png" });
    var URL = window.URL || window.webkitURL;
    var url = URL.createObjectURL(blob);

    /* return image url or add to map */

```

```c
#pragma pack(push, 1)
    typedef struct {
        unsigned short year;
        unsigned char mon;
        unsigned char day;
        unsigned char hour;
        unsigned char min;
        unsigned char sec;
        unsigned short msec;
    } frame_timestamp_st, *frame_timestamp_t;

    typedef struct _frame_head_st frame_head_st, * frame_head_t;
    struct _frame_head_st {
        unsigned int	leader:32;
        unsigned short	ver:16;
        unsigned int	stc:32;
        frame_timestamp_st ts;
        unsigned int	pl:32;
        unsigned char	el:8;
    };

    typedef struct _payload_head_st payload_head_st, *payload_head_t;
    struct _payload_head_st {
        unsigned char	dt:8;  //36 = heatmap image
        unsigned int	dl:32;
    };
    typedef struct _hub_image_item_st {
        int    width;   /**< Width in pixels of the location image. */
        int    height;  /**< Height in pixels of the location image.  */
        int    numBytes;/**< Size of the image in bytes.  */

        double north;   /**< Latitude of top boundary */
        double south;   /**< Latitude of bottom boundary */
        double east;    /**< Longitude of right boundary */
        double west;    /**< Longitude of left boundary */
        double altitude;  /**< Altitude in meters */
    }hub_image_item_st, *hub_image_item_t;

    typedef struct _heatmap_st {
        frame_head_st		frame;
        payload_head_st		payload;
        hub_image_item_st   head;
        void* png;
    } heatmap_cache_st,*heatmap_cache_t;
#pragma pack(pop)

```
