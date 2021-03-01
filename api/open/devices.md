### 拉取设备列表

**URI:**

    https://open.sodalife.xyz/iot/devices/

**请求方式：**

    GET

**请求参数 Query:**

| 参数         | 说明                                         | 是否必填            |
|--------------|--------------------------------------------|--------------------|
| access_token | 应用授权凭证                                 | 必填                |
| merchant_id  | [iot 所属商户 id](api/open/iot_merchants.md) | 否                  |
| limit        | 返回最多数量                                 | 否，默认值/最大值 10 |
| offset       | 列表起始位                                   | 否，默认 0           |

**授权范围：**

    iot:public

**返回：**

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "objects": [
      {
        "serial": "SD66790830",
        "feature": "SHOWER",
        "status": "AVAILABLE",
        "floor": 1,
        "position_number": 2,
        "online": true
      }
    ],
    "pagination": {
      "from": 0,
      "to": 1,
      "total": 100
    }
  }
  ```

- 异常时，接口将返回 [异常 JSON 数据](https://docs-open.sodalife.cc/#/api/error)

**返回参数：**

| 参数                       | 说明                                            |
|----------------------------|-----------------------------------------------|
| objects[0].serial          | 设备编号                                        |
| objects[0].feature         | [设备特性](concept/iot.md?id=Device.feature)    |
| objects[0].status          | [设备当前状态](concept/iot.md?id=Device.status) |
| objects[0].online          | 设备在线情况（true 在线，false 掉线）              |
| objects[0].floor           | 设备所在楼层                                    |
| objects[0].position_number | 设备位置编号                                    |
| pagination.from            | 分页起始位                                      |
| pagination.to              | 分页结束位                                      |
| pagination.total           | 总数                                            |

**示例：**

```bash

curl https://open.sodalife.xyz/iot/devices/?access_token=d86c828583c5c6160e8acfee88ba1590&limit=10&offset=0

#  {
#    "objects": [
#      {
#        "serial": "SD66790830",
#        "feature": "SHOWER",
#        "status": "AVAILABLE",
#        "floor": 1,
#        "position_number": 2,
#        "online": true
#      }
#    ],
#    "pagination": {
#      "from": 0,
#      "to": 1,
#      "total": 100
#    }
#  }
```

### 拉取设备详情

**URI:**

    https://open.sodalife.xyz/iot/devices/:serial

**请求方式：**

    GET

**请求参数 Query:**

| 参数         | 说明         | 是否必填 |
|--------------|------------|--------|
| access_token | 应用授权凭证 | 必填     |

**请求参数 Params:**

| 参数   | 说明     | 是否必填 |
|--------|--------|--------|
| serial | 设备编码 | 必填     |

**授权范围：**

    iot:public

**返回：**

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "serial": "SD66790830",
    "feature": "SHOWER",
    "modes": [
      {
        "preset": "DRINKING_HOT",
        "name": "热水",
        "value": 0.40,
        "unit": "升"
      },
      {
        "preset": "DRINKING_COLD",
        "name": "常温水",
        "value": 0.30,
        "unit": "升"
      }
    ],
    "online": true,
    "status": "AVAILABLE",
    "floor": 1,
    "position_number": 2,
    "merchant_id": "s78y55HHcxyT",
  }
  ```

- 异常时，接口将返回 [异常 JSON 数据](https://docs-open.sodalife.cc/#/api/error)

**返回参数：**

| 参数                 | 说明                                            |
|----------------------|-----------------------------------------------|
| serial               | 设备编号                                        |
| feature              | [设备特性](concept/iot.md?id=Device.feature)    |
| status               | [设备当前状态](concept/iot.md?id=Device.status) |
| online               | 设备在线情况（true 在线，false 掉线）              |
| modes                | 设备模式列表                                    |
| floor                | 设备所在楼层                                    |
| position_number      | 设备位置编号                                    |
| merchant_id          | [iot 所属商户 id](api/open/iot_merchants.md)    |
| modes[0].preset      | 预设模式                                        |
| modes[0].name        | 模式名                                          |
| modes[0].value       | 模式单价，单位：元                                |
| modes[0].unit        | 模式单位                                        |
| modes[0].description | 模式描述                                        |

**示例：**

```bash

curl https://open.sodalife.xyz/iot/devices/SD66790830?access_token=d86c828583c5c6160e8acfee88ba1590

# {
#   "serial": "SD66790830",
#   "feature": "SHOWER",
#   "modes": [
#     {
#       "preset": "DRINKING_HOT",
#       "name": "热水",
#       "value": 15,
#       "unit": "升",
#       "description": "",
#     },
#     {
#       "preset": "DRINKING_COLD",
#       "name": "常温水",
#       "value": 15,
#       "unit": "升",
#       "description": "",
#     }
#   ],
#   "online": true,
#   "status": "AVAILABLE",
#    "floor": 1,
#    "position_number": 2,
#    "merchant_id": "s78y55HHcxyT"
# }
```

### 联网设备开关

> 注：向联网设备发起开、关设备请求，区别于激活请求

**URI:**

    https://open.sodalife.xyz/iot/devices/:serial/switch

**请求方式：**

    POST

**请求参数 Query:**

| 参数         | 说明         | 是否必填 |
|--------------|------------|--------|
| access_token | 应用授权凭证 | 必填     |

**请求参数 Body (JSON):**

| 参数   | 说明                          | 是否必填 |
|--------|-----------------------------|--------|
| action | 设备开关动作（on 开启，off 关闭 | 必填     |

**授权范围：**

    iot:premier

**返回：**

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "status": "OK"
  }
  ```

- 异常时，接口将返回 [异常 JSON 数据](https://docs-open.sodalife.cc/#/api/error)

**返回参数：**

| 参数   | 说明     |
|--------|--------|
| status | 操作成功 |

**示例：**

```bash

curl -X POST \
  -H "Content-Type: application/json" \
  -d \
  '{
    "action": "off"
  }'\
  https://open.sodalife.xyz/iot/devices/:serial/switch?access_token=d86c828583c5c6160e8acfee88ba1590

# {
#   "status": "OK"
# }
```

### 联网设备异常上报

> 注：**应用**（对接方）需提供 url 地址负责接收 **苏打开放平台** 联网设备异常信息上报，具体消息格式及解析和验签规则见[webhook 说明](https://docs-open.sodalife.cc/#/api/webhook)

#### 异常事件

| 事件标识             | 描述                |
|----------------------|-------------------|
| IOT_ANOMALY:ABNORMAL | 设备异常上报 - 异常 |

##### 设备异常上报 - 异常

**消息格式**

| 参数                                              | 类型          | 含义                                            |
|---------------------------------------------------|---------------|-----------------------------------------------|
| signed_request                                    | SignedRequest | 已签名请求                                      |
| signed_request.event                              | string        | 事件，始终为 `IOT_ANOMALY:ABNORMAL`              |
| signed_request.iot_anomaly                        | object        | 设备异常详情                                    |
| signed_request.iot_anomaly.msg_id                 | string        | 通知消息 id                                     |
| signed_request.iot_anomaly.status                 | string (enum) | [设备异常值](concept/iot.md?id=Anomaly.status)  |
| signed_request.iot_anomaly.description            | string        | 设备异常描述                                    |
| signed_request.iot_anomaly.device                 | object        | 设备详情                                        |
| signed_request.iot_anomaly.device.serial          | string        | 设备编号                                        |
| signed_request.iot_anomaly.device.feature         | string        | [设备特性](concept/iot.md?id=Device.feature)    |
| signed_request.iot_anomaly.device.status          | string        | [设备当前状态](concept/iot.md?id=Device.status) |
| signed_request.iot_anomaly.device.online          | boolean       | 设备在线情况（true 在线，false 掉线）              |
| signed_request.iot_anomaly.device.floor           | number (int)  | 设备所在楼层                                    |
| signed_request.iot_anomaly.device.position_number | number (int)  | 设备位置编号                                    |
| signed_request.iot_anomaly.device.merchant_id     | string        | [iot 所属商户 id](api/open/iot_merchants.md)    |

**回调通知示例：**
测试 secret:

```bash
qvXxPd96cMZUzBTVnxt7b9QTrjtW5
```

Request:

```http
POST: ${client_iot_anomaly_url} 

signed_request=FgzXCWQ9A2n3V_AurY1NMhbizOFC647hVz4KFsUyPd4.eyJldmVudCI6IklPVF9BTk9NQUxZOkFCTk9STUFMIiwiaW90X2Fub21hbHkiOnsiZGVzY3JpcHRpb24iOiJTT1Mg5oql6K2mIiwiZGV2aWNlIjp7ImZlYXR1cmUiOiJTSE9XRVIiLCJmbG9vciI6MSwibWVyY2hhbnRfaWQiOiJzNzh5NTVISGN4eVQiLCJvbmxpbmUiOnRydWUsInBvc2l0aW9uX251bWJlciI6Miwic2VyaWFsIjoiU0Q2Njc5MDgzMCIsInN0YXR1cyI6IkFWQUlMQUJMRSJ9LCJtc2dfaWQiOiJuQTFaeUZMZDFqTWF2aUdzY1MxNlRGSlVsc1VBaCIsInN0YXR1cyI6IlNPUyJ9fQ
```

Response:

```http
HTTP/1.1 200 OK

success
```
