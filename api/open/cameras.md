## 视频API
接口说明
- 1.平台已经添加过设备信息，正常使用实时预览功能前需要关闭对应设备端视频加密配置选项。
- 2.调用所有接口需要先获取access_token凭证，具体参考[获取凭证](https://docs-open.sodalife.cc/#/guide/auth)
- 3.单台设备相关接口调用频率间隔建议间隔4秒以上，避免频繁调用。

### 拍照

接口说明
> 该接口用于手动触发设备拍照，返回图片的地址，拍照时间为触发手动命令的时间。

接口地址
> POST /iot/cameras/capture

数据提交方式
> application/json

请求参数

|参数|数据类型|是否必须|描述|
|---|---|---|---|
|access_token|string|true|授权凭证|
|device_serial|string|true|设备编号|

请求参数示例
```json
{
    "access_token":"ks0a9asfa9yaanfsahj9fa",
    "device_serial": "TEST0001"
}
```

返回参数

|参数|类型|描述|
|---|---|---|
|picUrl|string|图片url，图片保存有效期为2小时|

返回参数示例
```json
{
    "picUrl": "http://1.1.1.1:6040/pic?=d4=i535z4165s08d-=64idm*ep=t6p7i=d1s*i7d9i*d2=*9b8d0eaa5-16c2d72-4a555f-88i4e6*e2e28d4"
}
```

### 视频控制

接口说明
> 根据设备编号进行云台操作控制，开始云台控制之后必须先调用停止云台控制才能进行其他操作

接口地址
> POST /iot/cameras/controlling

数据提交方式
> application/json

请求参数

|参数|数据类型|是否必须|描述|
|---|---|---|---|
|access_token|string|true|授权凭证|
|device_serial|string|true|设备编号|
|action|integer|true|0-开始 ，1-停止|
|command|string|true|8-放大，9-缩小，10-近焦距，11-远焦距|

> 保留指令集：0-上，1-下，2-左，3-右，4-左上，5-左下，6-右上，7-右下

请求参数示例
```json
{
    "access_token":"ks0a9asfa9yaanfsahj9fa",
    "device_serial": "TEST0001",
    "action": 1,
    "command": "8"
}
```

返回参数

|参数|类型|描述|
|---|---|---|
|data|string|返回数据|

返回参数示例
```json
{
    "data": "SUCCESS"
}
```


### 下发语音

接口说明
> 下发自定义语音到设备，音频文件支持wav、mp3、aac格式，最大20M。

接口地址
> POST /iot/cameras/sendvoice

数据提交方式
> multipart/form-data

请求参数

|参数|数据类型|是否必须|描述|
|---|---|---|---|
|access_token|string|true|授权凭证|
|device_serial|string|true|设备编号|
|voice_file|file|true|音频文件|

返回参数

|参数|类型|描述|
|---|---|---|
|data|string|返回数据|

返回参数示例
```json
{
    "data": "SUCCESS"
}
```

### 直播地址

接口说明
> 获取设备的直播地址信息，H5视频插件接入直播地址可参考:[UIKit Javascript](http://open.ys7.com/doc/zh/uikit/uikit_javascript.html)

接口地址
> POST /iot/cameras/liveurl

数据提交方式
> applicatiion/json

请求参数

|参数|类型|是否必须|描述|
|---|---|---|---|
|access_token|string|true|授权凭证|
|device_serial|string|true|设备唯一标识|
|protocol|string|false|支持hls、ezopen两种协议地址，默认hls|

请求参数示例

```json
{
    "access_token":"ks0a9asfa9yaanfsahj9fa",
    "device_serial": "748d84750e3a4a5bbad3cd4af9ed5101",
    "protocol": "hls"
}
```

返回参数

|参数|类型|描述|
|---|---|---|
|url|string|直播地址URL|

返回参数示例
```json
{
    "url": "http://hls.open.ys7.com/openlive/dasd8a8f8da8fsa87s7s.hd.m3u8"
}
```

## 注：异常
接口在遇到异常时，平台统一返回指定格式的错误数据。
异常数据格式如下
```json
{
  "error": "错误码",
  "error_description": "错误描述"
}
```

通用异常

|异常机器码(error)|异常描述(error_description)|
|---|---|
|INTERNAL_SERVER_ERROR|服务内部异常|
|TEMPORARILY_UNAVAILABLE|服务暂时无法访问|
|TOO_MANY_REQUESTS|请求频率过高|
|NOT_FOUND|找不到服务|
|INVALID_RESOURCE|资源无效（如找不到资源）|
|INVALID_REQUEST|请求不合法（如参数错误）|
|UNAUTHORIZED_CLIENT|应用未有权限做该操作(如应用未有获取授权凭证的权限)|
|ACCESS_DENIED|用户或授权服务器拒绝授予数据访问权限|
|LOCKED|资源已被锁定|
|GONE|资源已失效|
|CANCELLED|操作已被取消|
|COMPLETED|操作已被完成|
|UNAUTHORIZED|当前会话无效（如已过期）|
|UNAUTHORIZED_USER|当前会话用户无效（如未登录）|
|EXTERNAL_SERVER_ERROR|外部服务异常|

授权异常码

|异常机器码(error)|异常描述(error_description)|
|---|---|
|INVALID_CLIENT|client_id或client_secret参数无效|
|INVALID_GRANT|提供的AccessGrant(codeortoken)是无效的、过期的或已撤销的|
|INVALID_SCOPE|请求的scope无效|
|INVALID_REDIRECT_URI|提供的重定向地址与白名单不匹配|
|UNSUPPORTED_RESPONSE_TYPE|不支持的ResponseType|
|UNSUPPORTED_GRANT_TYPE|不支持的GrantType|

视频业务错误码

|异常机器码(error)|异常描述(error_description)|消除建议|
|---|---|---|
|10001|参数错误|参数为空或格式不正确|
|10002|access_token异常或过期|重新获取access_token|
|10005|appKey异常|appKey被冻结|
|10051|无权限进行抓图|设备不属于当前用户或者未分享给当前用户|
|20002|设备不存在||
|20006|网络异常|检查设备网络状况，稍后再试|
|20007|设备不在线|检查设备是否在线|
|20008|设备响应超时|操作过于频繁或者设备不支持萤石协议抓拍|
|20014|device_serial不合法||
|49999|数据异常|接口调用异常|
|60017|设备抓图失败|设备返回失败|
|60020|不支持该命令|确认设备是否支持抓图|
|20008|设备响应超时|操作过于频繁，稍后再试|
|60000|设备不支持云台控制||
|60001|用户无云台控制权限||
|60006|云台当前操作失败|稍候再试|
|60009|正在调用预置点||
|60020|不支持该命令|确认设备是否支持该操作|
|111001|语音文件格式错误||
|111002|语音文件时长不合法||
|111003|语音文件上传失败||
|111004|语音文件转换失败||
|111005|语音文件时长获取||
|111006|语音文件列表获取失败||
|111007|下发的语音文件URL不存在||
|111008|参数错误，语音文件不能为空||
|111009|参数错误，语音文件URL不能为空||
|111010|参数错误，设备序列号不能为空||