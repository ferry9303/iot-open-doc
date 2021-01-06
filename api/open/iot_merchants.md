### 查询 IoT 设备所属商户列表

**URI:**

    https://open.sodalife.xyz/iot/merchants/

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

**返回：**

- 正常时，接口将返回以下 JSON 数据：

  ```json
  {
    "objects": [
      {
        "id": "MERCHANT_ID",
        "name": "商户 A"
      },
      {
        "id": "MERCHANT_ID",
        "name": "商户 B"
      }
    ],
    "pagination": {
      "from": 0,
      "to": 2,
      "total": 2
    }
  }
  ```

- 异常时，接口将返回 [异常 JSON 数据](https://docs-open.sodalife.cc/#/api/error)
