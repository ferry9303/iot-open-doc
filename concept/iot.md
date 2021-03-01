# IoT 概念参考

## 枚举数据 ENUM

### Device.feature

| 设备特性值 | 说明     |
|------------|--------|
| SHOWER     | 淋浴设备 |

### Device.status

| 设备状态值 | 说明   |
|------------|------|
| AVAILABLE  | 可使用 |
| USING      | 使用中 |
| ABNORMAL   | 异常   |
| LOCKED     | 锁定   |
| OCCUPIED   | 占用   |

### Occupation.status

| 占用状态值      | 说明     |
|-----------------|--------|
| OCCUPIED        | 已占用   |
| OCCUPY_FAILURE  | 占用失败 |
| RELEASED        | 已释放   |
| RELEASE_FAILURE | 释放失败 |

### Activation.status

| 激活状态值       | 说明           |
|------------------|--------------|
| WAITING          | 等待         ｜
| ACTIVED          | 激活成功       |
| ACTIVE_FAILURE   | 激活失败       |
| TERIMATED        | 终止（结算）成功 |
| TERIMATE_FAILURE | 终止（结算）失败 |

### Anomaly.status

| 设备异常值 | 说明     |
|------------|--------|
| SOS        | SOS 报警 |
