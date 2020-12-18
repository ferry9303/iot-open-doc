### 拉取设备列表

**URI:**

    https://open.sodalife.xyz/iot/devices/

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
| objects[0].feature    | [设备特性](concept/iot.md?id=Device.feature) |
| objects[0].status     | [设备当前状态](concept/iot.md?id=Device.status) |
| objects[0].online     | 设备在线情况（true 在线，false 掉线）  |
| pagination.from       | 分页起始位                          |
| pagination.to         | 分页结束位                          |
| pagination.total      | 总数                               |

**示例：**

```bash

curl https://open.sodalife.xyz/iot/devices/?access_token=d86c828583c5c6160e8acfee88ba1590&limit=10&offset=0

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
| access_token | 应用授权凭证   | 必填             |

**请求参数 Params:**

| 参数                  | 说明          | 是否必填    |
| --------------------- | ------------- | -------- |
| serial               | 设备编码       | 必填       |

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
| feature               | [设备特性](concept/iot.md?id=Device.feature) |
| status                | [设备当前状态](concept/iot.md?id=Device.status) |
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

### 拉取设备占用列表

**URI:**

    https://open.sodalife.xyz/iot/devices/:serial/occupation/

**请求方式：**

    GET

**请求参数 Query:**

| 参数         | 说明         | 是否必填          |
| ------------ | ------------ | --------------- |
| access_token | 应用授权凭证 | 必填               |
| limit        | 返回最多数量 | 否，默认值/最大值 10 |
| offset       | 列表起始位   | 否，默认 0          |

**请求参数 Params:**

| 参数                  | 说明          | 是否必填    |
| --------------------- | ------------- | -------- |
| serial               | 设备编码       | 必填       |

**授权范围：**

    iot:control

**返回: ***

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "objects": [
      {
        "id": "OCCUPATION_ID",
        "serial": "SD66790830",
        "status": "OCCUPIED",
        "created_at": "2020-12-18T00:09:00+08:00",
        "start_at": "2020-12-19T00:33:00+08:00",
        "end_at": "2020-12-19T01:33:00+08:00"
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
| objects[0].id         | 设备占用 id                        |
| objects[0].serial     | 设备编号                           |
| objects[0].status     | [占用状态](concept/iot.md?id=Occupation.status) |
| objects[0].created_at | 创建时间。格式为 ISO8601。以苏打平台系统时间为准。   ｜
| objects[0].status_at  | 占用开始时间。格式为 ISO8601。以苏打平台系统时间为准。｜
| objects[0].end_at     | 占用结束时间。格式为 ISO8601。以苏打平台系统时间为准。｜
| pagination.from       | 分页起始位                          |
| pagination.to         | 分页结束位                          |
| pagination.total      | 总数                               |

**示例：**

```bash

curl ttps://open.sodalife.xyz/iot/devices/SD66790830/occupation/?access_token=d86c828583c5c6160e8acfee88ba1590&limit=10&offset=0

# {
#     "objects": [
#       {
#         "id": "201217115931083586",
#         "serial": "SD66790830",
#         "status": "OCCUPIED",
#         "created_at": "2020-12-18T00:09:00+08:00",
#         "start_at": "2020-12-19T00:33:00+08:00",
#         "end_at": "2020-12-19T01:33:00+08:00"
#       }
#     ],
#     "pagination": {
#       "from": 0,
#       "to": 1,
#       "total": 100
#     }
#   }
```

### 占用设备

**URI:**

    https://open.sodalife.xyz/iot/occupation/

**请求方式：**

    POST

**请求参数 Query:**

| 参数         | 说明         | 是否必填          |
| ------------ | ------------ | --------------- |
| access_token | 应用授权凭证 | 必填               |

**请求参数 Body (JSON):**

| 参数                  | 说明          | 是否必填                    |
| --------------------- | ------------- | ------------------------ |
｜ serial               | 设备编号       | 必填
| user_id               | 应用用户 id     | 必填（user_id 或 mobile 二选一）    |
| mobile                | 用户手机号      | 否 (iot:premier 权限可用) |
| start_at       | 开始时间。格式为 ISO8601。以苏打平台系统时间为准。      | 必填                    ｜
| end_at         | 结束时间。格式为 ISO8601。以苏打平台系统时间为准。      | 必填                     |

**授权范围：**

    iot:control

**返回: ***

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "id": "OCCUPATION_ID",
    "serial": "SD66790830",
    "status": "OCCUPIED",
    "created_at": "2020-12-18T00:09:00+08:00"
  }
  ```

- 异常时，接口将返回 [异常 JSON 数据](http://docs.open.sodalife.cc/#/api/error)

**返回参数：**

| 参数                   | 说明                              |
| --------------------- | --------------------------------- |
| id                    | 占用设备 ID，最大长度 64 字节         |
｜ serial               ｜ 设备编号                           |
| status                | [占用状态](concept/iot.md?id=Occupation.status)  ｜
| created_at           | 创建时间。格式为 ISO8601。以苏打平台系统时间为准。      |

**示例：**

```bash

curl -X POST \
  -H "Content-Type: application/json" \
  -d \
  '{
    "serial": "SD66790830",
    "user_id": "1b9efbab658fbd15557cf1c43bb89ff2",
    "start_at": "2020-12-19T00:33:00+08:00",
    "end_at": "2020-12-19T01:33:00+08:00"
  }'\
  https://open.sodalife.xyz/iot/occupation/?access_token=d86c828583c5c6160e8acfee88ba1590

#  {
#    "id": "201217115931083586",
#    "serial": "SD66790830",
#    "status": "OCCUPIED",
#    "created_at": "2020-12-18T00:09:00+08:00"
#  }
```

### 取消占用设备

**URI:**

    https://open.sodalife.xyz/iot/occupation/:id/actions/release

**请求方式：**

    POST

**请求参数 Query:**

| 参数         | 说明         | 是否必填          |
| ------------ | ------------ | --------------- |
| access_token | 应用授权凭证 | 必填               |

**请求参数 Params:**

| 参数                  | 说明          | 是否必填    |
| --------------------- | ------------- | -------- |
| id                    | 设备占用 id     | 必填       |

**授权范围：**

    iot:control

**返回: ***

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "id": "OCCUPATION_ID",
    "serial": "SD66790830",
    "status": "RELEASED",
    "created_at": "2020-12-18T00:09:00+08:00",
    "released_at": "2020-12-18T00:19:00+08:00"
  }
  ```

- 异常时，接口将返回 [异常 JSON 数据](http://docs.open.sodalife.cc/#/api/error)

**返回参数：**

| 参数                   | 说明                              |
| --------------------- | --------------------------------- |
| id                    | 占用设备 ID，最大长度 64 字节         |
｜ serial               ｜ 设备编号                           |
| status                | [占用状态](concept/iot.md?id=Occupation.status)  |
| created_at           | 创建时间。格式为 ISO8601。以苏打平台系统时间为准。      |
| released_at           | 取消占用时间。格式为 ISO8601。以苏打平台系统时间为准。  |

**示例：**

```bash

curl -X POST \
  -H "Content-Type: application/json" \
  https://open.sodalife.xyz/iot/occupation/201217115931083586/actions/release?access_token=d86c828583c5c6160e8acfee88ba1590

#  {
#    "id": "201217115931083586",
#    "serial": "SD66790830",
#    "status": "RELEASED",
#    "created_at": "2020-12-18T00:09:00+08:00",
#    "released_at": "2020-12-18T00:19:00+08:00"
#  }
```
