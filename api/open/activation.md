### 激活联网设备

**URI:**

    https://open.sodalife.xyz/iot/activation/

**请求方式：**

    POST

**请求参数 Query:**

| 参数         | 说明         | 是否必填 |
|--------------|------------|--------|
| access_token | 应用授权凭证 | 必填     |

**请求参数 Body (JSON):**

| 参数             | 说明                                                                     | 是否必填                     |
|------------------|------------------------------------------------------------------------|----------------------------|
| client_order_id  | 应用内部的订单 ID，同一应用下不允许重复，最大长度 64 字节。                 | 必填                         |
| device.serial    | 设备编号                                                                 | 必填                         |
| device.auth_code | 设备鉴权码                                                               | 必填（从设备二维码中解析获取） |
| modes[0].preset  | 激活设备所对应的预设模式                                                 | 必填                         |
| auth_hold.amount | 先享后付设备单次最高可用金额，单位为元                                    | 否，默认单次最高可用 100 元   |
| webhook_url      | 应用异步接收苏打平台服务器通知设备状态及消费详情的通知地址               | 必填                         |
| attachment       | 自定义附加数据，最长支持 128 字节。在查询 API 和 Webhook 通知中将原样返回。 | 否，默认为空字符串            |

**授权范围：**

    iot:control

**返回：**

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "id": "ACTIVATION_ID",
    "status": "WAITING"
  }
  ```

- 异常时，接口将返回 [异常 JSON 数据](https://docs-open.sodalife.cc/#/api/error)

**返回参数：**

| 参数   | 说明                                                |
|--------|---------------------------------------------------|
| id     | 激活设备 ID，最大长度 64 字节                        |
| status | [激活设备状态](concept/iot.md?id=Activation.status) |

**示例：**

```bash

curl -X POST \
  -H "Content-Type: application/json" \
  -d \
  '{
    "client_order_id": "xxx",
    "device": {
      "serial": "SD66790830",
      "auth_code": "zZONoj90"
    },
    "modes": [
      {
        "preset": "DRINKING_HOT"
      },
      {
        "preset": "DRINKING_COLD"
      }
    ],
    "auth_hold": {
      "amount": 1.00
    },
    "webhook_url": "https://api.sodalife.xyz/v1/iot/activation/webhook/"
  }'\
  https://open.sodalife.xyz/iot/activation/?access_token=d86c828583c5c6160e8acfee88ba1590

#  {
#    "id": "202117115931093026"
#  }
```

> 注：设备是否激活成功根据异步通知 webhook_url 结果判断，具体消息格式及解析和验签规则见[webhook 说明](https://docs-open.sodalife.cc/#/api/webhook)

#### Webhook 激活事件

| 事件标识                         | 描述                      |
|----------------------------------|-------------------------|
| IOT_ACTIVATION:ACTIVED           | 设备激活 - 激活成功       |
| IOT_ACTIVATION:ACTIVE_FAILURE    | 设备激活 - 激活失败       |
| IOT_ACTIVATION:TERMINATED        | 设备终止 - 已终止（结算）   |
| IOT_ACTIVATION:TERMINATE_FAILURE | 设备终止 - 终止（结算）失败 |

##### 设备激活 - 激活成功

**消息格式**

| 参数                                               | 类型             | 含义                                 |
|----------------------------------------------------|------------------|------------------------------------|
| signed_request                                     | SignedRequest    | 已签名请求                           |
| signed_request.event                               | string           | 事件，始终为 `IOT_ACTIVATION:ACTIVED` |
| signed_request.iot_activation                      | object           | 设备激活详情                         |
| signed_request.iot_activation.id                   | string           | 设备激活 ID                          |
| signed_request.iot_activation.client_order_id      | string           | 应用内部的激活设备 ID                |
| signed_request.iot_activation.modes                | array (object)   | 设备模式详情                         |
| signed_request.iot_activation.modes[0].preset      | string           | 设备预设模式                         |
| signed_request.iot_activation.modes[0].name        | string           | 模式名                               |
| signed_request.iot_activation.modes[0].value       | number (float)   | 模式单价，单位：元                     |
| signed_request.iot_activation.modes[0].unit        | string           | 模式单位                             |
| signed_request.iot_activation.modes[0].description | string           | 模式描述                             |
| signed_request.iot_activation.attachment           | string           | 激活设备的自定义附加数据             |
| signed_request.iot_activation.actived_at           | string (ISO8601) | 激活时间                             |
| signed_request.iot_activation.status               | string (enum)    | 激活设备状态                         |

##### 设备激活 - 激活失败

**消息格式**

| 参数                                               | 类型           | 含义                                        |
|----------------------------------------------------|----------------|-------------------------------------------|
| signed_request                                     | SignedRequest  | 已签名请求                                  |
| signed_request.event                               | string         | 事件，始终为 `IOT_ACTIVATION:ACTIVE_FAILURE` |
| signed_request.iot_activation                      | object         | 设备激活详情                                |
| signed_request.iot_activation.id                   | string         | 设备激活 ID                                 |
| signed_request.iot_activation.client_order_id      | string         | 应用内部的激活设备 ID                       |
| signed_request.iot_activation.modes                | array (object) | 设备模式详情                                |
| signed_request.iot_activation.modes[0].preset      | string         | 设备预设模式                                |
| signed_request.iot_activation.modes[0].name        | string         | 模式名                                      |
| signed_request.iot_activation.modes[0].value       | number (float) | 模式单价，单位：元                            |
| signed_request.iot_activation.modes[0].unit        | string         | 模式单位                                    |
| signed_request.iot_activation.modes[0].description | string         | 模式描述                                    |
| signed_request.iot_activation.attachment           | string         | 激活设备的自定义附加数据                    |
| signed_request.iot_activation.status               | string (enum)  | 激活设备状态                                |

##### 设备终止 - 已终止（结算）

**消息格式**

| 参数                                                    | 类型             | 含义                                     |
|---------------------------------------------------------|------------------|----------------------------------------|
| signed_request                                          | SignedRequest    | 已签名请求                               |
| signed_request.event                                    | string           | 事件，始终为 `IOT_ACTIVATION:TERMINATED`  |
| signed_request.iot_activation                           | object           | 设备激活详情                             |
| signed_request.iot_activation.id                        | string           | 设备激活 ID                              |
| signed_request.iot_activation.client_order_id           | string           | 应用内部的激活设备 ID                    |
| signed_request.iot_activation.modes                     | array (object)   | 设备模式详情                             |
| signed_request.iot_activation.modes[0].preset           | string           | 设备预设模式                             |
| signed_request.iot_activation.modes[0].name             | string           | 模式名                                   |
| signed_request.iot_activation.modes[0].value            | number (float)   | 模式单价，单位：元                         |
| signed_request.iot_activation.modes[0].unit             | string           | 模式单位                                 |
| signed_request.iot_activation.modes[0].description      | string           | 模式描述                                 |
| signed_request.iot_activation.modes[0].used             | object           | 该模式下的设备使用详情                   |
| signed_request.iot_activation.modes[0].used.amount      | number (float)   | 已使用金额，单位：元                       |
| signed_request.iot_activation.modes[0].used.flow_amount | number (float)   | 已使用流量，单位见 modes[0].unit 返回的值 |
| signed_request.iot_activation.modes[0].used.duration    | number (float)   | 已使用时长，单位：秒                       |
| signed_request.iot_activation.attachment                | string           | 激活设备的自定义附加数据                 |
| signed_request.iot_activation.actived_at                | string (ISO8601) | 激活时间                                 |
| signed_request.iot_activation.terminated_at             | string (ISO8601) | 终止时间                                 |
| signed_request.iot_activation.status                    | string (enum)    | 激活设备状态                             |

##### 设备终止 - 终止（结算）失败

**消息格式**

| 参数                                               | 类型             | 含义                                           |
|----------------------------------------------------|------------------|----------------------------------------------|
| signed_request                                     | SignedRequest    | 已签名请求                                     |
| signed_request.event                               | string           | 事件，始终为 `IOT_ACTIVATION:TERMINATE_FAILURE` |
| signed_request.iot_activation                      | object           | 设备激活详情                                   |
| signed_request.iot_activation.id                   | string           | 设备激活 ID                                    |
| signed_request.iot_activation.client_order_id      | string           | 应用内部的激活设备 ID                          |
| signed_request.iot_activation.modes                | array (object)   | 设备模式详情                                   |
| signed_request.iot_activation.modes[0].preset      | string           | 设备预设模式                                   |
| signed_request.iot_activation.modes[0].name        | string           | 模式名                                         |
| signed_request.iot_activation.modes[0].value       | number (float)   | 模式单价，单位：元                               |
| signed_request.iot_activation.modes[0].unit        | string           | 模式单位                                       |
| signed_request.iot_activation.modes[0].description | string           | 模式描述                                       |
| signed_request.iot_activation.attachment           | string           | 激活设备的自定义附加数据                       |
| signed_request.iot_activation.actived_at           | string (ISO8601) | 激活时间                                       |
| signed_request.iot_activation.status               | string (enum)    | 激活设备状态                                   |

### 查询联网设备激活详情

**URI:**

    https://open.sodalife.xyz/iot/activation/query

**请求方式：**

    GET

**请求参数 Query:**

| 参数          | 说明         | 是否必填 |
|---------------|------------|--------|
| access_token  | 应用授权凭证 | 必填     |
| activation_id | 激活设备 id  | 必填     |

**授权范围：**

    iot:control

**返回：**

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "id": "ACTIVATION_ID",
    "client_order_id": "xxx",
    "modes": [
      {
        "preset": "DRINKING_HOT",
        "name": "热水",
        "value": 0.40,
        "unit": "升",
        "description": "",
        "used": {
          "amount": 1.20 ,
          "flow_amount": 20,
          "duration": 3600
        }
      }
    ],
    "attachment": "",
    "actived_at": "2021-01-27T00:09:00+08:00",
    "terminated_at": "2021-01-27T00:10:10+08:00",
    "status": "TERIMATED"
  }
  ```

- 异常时，接口将返回 [异常 JSON 数据](https://docs-open.sodalife.cc/#/api/error)

**返回参数：**

| 参数                      | 说明                                                |
|---------------------------|---------------------------------------------------|
| id                        | 设备激活 ID                                         |
| client_order_id           | 应用内部的激活设备 ID                               |
| modes                     | 设备模式详情                                        |
| modes[0].preset           | 设备预设模式                                        |
| modes[0].name             | 模式名                                              |
| modes[0].value            | 模式单价，单位：元                                    |
| modes[0].unit             | 模式单位                                            |
| modes[0].description      | 模式描述                                            |
| modes[0].used             | 该模式下的设备使用详情                              |
| modes[0].used.amount      | 已使用金额，单位：元                                  |
| modes[0].used.flow_amount | 已使用流量，单位见 modes[0].unit 返回的值            |
| modes[0].used.duration    | 已使用时长，单位：秒                                  |
| attachment                | 激活设备的自定义附加数据                            |
| actived_at                | 激活时间                                            |
| terminated_at             | 终止时间                                            |
| status                    | [激活设备状态](concept/iot.md?id=Activation.status) |

**示例：**

```bash

curl https://open.sodalife.xyz/iot/activation/query?access_token=d86c828583c5c6160e8acfee88ba1590&activation_id=202117115931093026

#  {
#    "id": "202117115931093026",
#    "client_order_id": "xxx",
#    "modes": [
#      {
#        "preset": "DRINKING_HOT",
#        "name": "热水",
#        "value": 0.40,
#        "unit": "升",
#        "description": "",
#        "used": {
#          "amount": 1.20 ,
#          "flow_amount": 20,
#          "duration": 3600
#         }
#       }
#    ],
#    "attachment": "",
#    "actived_at": "2021-01-27T00:09:00+08:00",
#    "terminated_at": "2021-01-27T00:10:10+08:00",
#    "status": "TERIMATED"
#  }
```

### 联网设备激活重试

**URI:**

    https://open.sodalife.xyz/iot/activation/:id/actions/retry

**请求方式：**

    POST

**请求参数 Query:**

| 参数         | 说明         | 是否必填 |
|--------------|------------|--------|
| access_token | 应用授权凭证 | 必填     |

**请求参数 Params:**

| 参数 | 说明        | 是否必填 |
|------|-----------|--------|
| id   | 激活设备 id | 必填     |

**授权范围：**

    iot:control

**返回：**

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "id": "ACTIVATION_ID",
    "status": "WAITING"
  }
  ```

- 异常时，接口将返回 [异常 JSON 数据](https://docs-open.sodalife.cc/#/api/error)

**返回参数：**

| 参数   | 说明                                                |
|--------|---------------------------------------------------|
| id     | 激活设备 ID，最大长度 64 字节                        |
| status | [激活设备状态](concept/iot.md?id=Activation.status) |

**示例：**

```bash

curl -X POST \
  -H "Content-Type: application/json" \
  https://open.sodalife.xyz/iot/activation/202117115931093026/actions/retry?access_token=d86c828583c5c6160e8acfee88ba1590

#  {
#    "id": "202117115931093026",
#    "status": "WAITING"
#  }
```

> 注：设备是否激活成功根据异步通知 webhook_url 结果判断

### 终止已激活的联网设备

> 注：先享后付设备的结算请求接口，先付后享设备不支持终止已激活设备

**URI:**

    https://open.sodalife.xyz/iot/activation/:id/actions/terminate 

**请求方式：**

    POST

**请求参数 Query:**

| 参数         | 说明         | 是否必填 |
|--------------|------------|--------|
| access_token | 应用授权凭证 | 必填     |

**请求参数 Params:**

| 参数 | 说明        | 是否必填 |
|------|-----------|--------|
| id   | 激活设备 id | 必填     |

**授权范围：**

    iot:control

**返回：**

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "id": "ACTIVATION_ID",
    "status": "TERMINATED"
  }
  ```

- 异常时，接口将返回 [异常 JSON 数据](https://docs-open.sodalife.cc/#/api/error)

**返回参数：**

| 参数   | 说明                                                |
|--------|---------------------------------------------------|
| id     | 激活设备 ID，最大长度 64 字节                        |
| status | [激活设备状态](concept/iot.md?id=Activation.status) |

**示例：**

```bash

curl -X POST \
  -H "Content-Type: application/json" \
  https://open.sodalife.xyz/iot/activation/202117115931093026/actions/terminate?access_token=d86c828583c5c6160e8acfee88ba1590

#  {
#    "id": "202117115931093026",
#    "status": "TERMINATED"
#  }
```
