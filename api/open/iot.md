### 拉取设备列表

**URI:**

    https://open.sodalife.xyz/iot/devices

**请求方式：**

    GET

**请求参数 Query:**

| 参数         | 说明         | 是否必填          |
| ------------ | ------------ | --------------- |
| access_token | 应用授权凭证 | 必填               |
| limit        | 返回最多数量 | 否，默认值/最大值 10 |
| offset       | 列表起始位   | 否，默认 0          |

**授权范围：**

    iot:public

**返回: ***

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "objects": [
      {
        "serial": "SD66790830",
        "feature": "SHOWER",
        "status": "AVAILABLE",
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

- 异常时，接口将返回 [异常 JSON 数据](http://docs.open.sodalife.cc/#/api/error)

**返回参数：**

| 参数                   | 说明                              |
| --------------------- | --------------------------------- |
| objects[0].serial     | 设备编号                           |
| objects[0].feature    | 设备特性     TODO                  |
| objects[0].status     | 设备当前状态  TODO                   |
| objects[0].online     | 设备在线情况（true 在线，false 掉线）  |
| pagination.from       | 分页起始位                          |
| pagination.to         | 分页结束位                          |
| pagination.total      | 总数                               |

**示例：**

```bash

curl https://open.sodalife.xyz/iot/devices?access_token=d86c828583c5c6160e8acfee88ba1590&limit=10&offset=0

#  {
#    "objects": [
#      {
#        "serial": "SD66790830",
#        "feature": "SHOWER",
#        "status": "AVAILABLE",
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

| 参数         | 说明         | 是否必填          |
| ------------ | ------------ | --------------- |
| access_token | 应用授权凭证 | 必填               |

**请求参数 Params:**

| 参数                  | 说明          | 是否必填    |
| --------------------- | ------------- | -------- |
| serial               | 设备编码      | 必填       |

**授权范围：**

    iot:public

**返回: ***

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
      "serial": "SD66790830",
      "feature": "SHOWER",
      "modes": [
        {
          "preset": "DRINKING_HOT",
          "name": "热水",
          "value": 15,
          "unit": "升",
          "description": "",
        },
        {
          "preset": "DRINKING_COLD",
          "name": "常温水",
          "value": 15,
          "unit": "升",
          "description": "",
        }
      ],
      "online": true,
      "status": "AVAILABLE"
    }
  ```

- 异常时，接口将返回 [异常 JSON 数据](http://docs.open.sodalife.cc/#/api/error)

**返回参数：**

| 参数                   | 说明                              |
| --------------------- | --------------------------------- |
| serial                | 设备编号                           |
| feature               | 设备特性     TODO                  |
| status                | 设备当前状态  TODO                   |
| online                | 设备在线情况（true 在线，false 掉线）  |
｜ modes                | 设备模式列表                         ｜
｜ modes[0].preset      | 预设模式                            |
｜ modes[0].name        | 模式名                              |
｜ modes[0].value       | 模式单价                            |
｜ modes[0].unit        | 模式单位                            |
｜ modes[0].description | 模式描述                            |

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
#   "status": "AVAILABLE"
# }
```

