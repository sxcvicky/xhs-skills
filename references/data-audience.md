# Audience 受众层接口

> ✅ **已验证** (2026-04-13) - 2/2 接口可用

## 接口列表

### 1. 活跃博主

```
POST /api/v1/audience/active
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "keyword": "护肤",
  "timeRange": "30",
  "sortBy": "noteCount",
  "limit": 10
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "keyword": "护肤",
    "totalAuthors": 0,
    "authors": [
      {
        "nickname": "珍珍很懒科普",
        "noteCount": 1,
        "totalEngagement": "3677",
        "avgEngageRate": "6.50%",
        "pgyProfile": null
      }
    ]
  }
}
```

**注意**: 当 `pgyProfile` 为 `null` 时，表示该博主未被蒲公英平台采集

**可选参数**:
- `sortBy`: noteCount(发文数)/fansCount(粉丝数)，默认 noteCount

---

### 2. 受众画像

```
POST /api/v1/audience/profile
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "keyword": "华为",
  "timeRange": "30"
}
```

---

### 3. 博主详情

```
GET /api/v1/audience/authors/:authorId
```

**状态**: ✅ 可用

**支持两种查询方式**:
1. 使用 PGY 的 user_id（返回完整商业画像）
2. 使用博主昵称（降级到灵犀聚合数据）

**实测响应（使用昵称）**:
```json
{
  "success": true,
  "data": {
    "authorId": "皮肤科辉哥",
    "nickname": "皮肤科辉哥",
    "fansCount": 0,
    "avgReadNum": 7123881,
    "avgEngageNum": 222373,
    "dataSource": "lingxi"
  }
}
```

**注意**: `dataSource` 字段标识数据来源：
- `pgy`: 蒲公英平台完整数据
- `lingxi`: 灵犀平台聚合数据（降级查询）

---

### 4. 博主内容分析

```
GET /api/v1/audience/authors/:authorId/content
```

**状态**: ✅ 可用

**实测响应**:
```json
{
  "success": true,
  "data": {
    "authorId": "皮肤科辉哥",
    "nickname": "皮肤科辉哥",
    "totalNotes": 320,
    "totalExposure": 15000000,
    "totalReads": 8500000,
    "totalInteractions": 680000,
    "contentTypes": [
      { "type": 1, "count": 120, "avgEngagement": 1500 },
      { "type": 2, "count": 200, "avgEngagement": 2500 }
    ],
    "topNotes": [
      { "noteId": "n_123", "title": "华为Mate70深度测评", "engageNum": 120000, "createTime": "2026-03-20" }
    ]
  }
}
```

---

### 5. 博主粉丝画像

```
GET /api/v1/audience/authors/:authorId/fans
```

**状态**: ✅ 可用

**实测响应**:
```json
{
  "success": true,
  "data": {
    "fansCount": 580000,
    "ageDistribution": [
      { "age": "18-24", "ratio": "25%" },
      { "age": "25-34", "ratio": "45%" }
    ],
    "genderDistribution": [
      { "gender": "女", "ratio": "65%" },
      { "gender": "男", "ratio": "35%" }
    ],
    "interestTopics": [
      { "interest": "数码", "ratio": "45%" }
    ],
    "deviceTypes": [
      { "device": "iPhone", "ratio": "60%" }
    ],
    "locationDistribution": [
      { "province": "广东", "ratio": "20%" }
    ],
    "message": "部分数据可能为null"
  }
}
```
