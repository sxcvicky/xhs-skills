# 博主数据接口

用于博主列表查询、粉丝画像分析、互动数据评估。

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## 博主查询

### 获取博主列表（飙升榜）

```
POST /api/v1/discovery/authors/rising
```

**参数**:
- `page`, `pageSize`: 分页
- `keyword` (string): 搜索关键词
- `fansMin` (number): 最小粉丝数

**响应**:
```json
{
  "success": true,
  "data": {
    "authors": [
      {
        "id": 1,
        "userId": "5a2c3e4f5b6c7d8e9f0a1b2c",
        "nickname": "护肤小达人",
        "fansCount": 1250000,
        "noteCount": 356,
        "likeAndCollectCount": 8900000
      }
    ]
  }
}
```

**使用场景**:
- 筛选合适合作博主
- 分析博主量级分布
- 计算平均互动率

---

## 受众画像

### 获取受众画像

```
POST /api/v1/audience/active
```

**参数**:
- `topicId` (number): 话题ID
- `authorId` (string): 博主ID

**响应**:
```json
{
  "code": 200,
  "data": {
    "demographics": {
      "gender": { "male": 25.5, "female": 74.5 },
      "age": { "18-24": 45.2, "25-34": 38.6 }
    },
    "interests": ["美妆", "护肤", "时尚"],
    "location": {
      "topProvinces": ["广东", "浙江", "江苏"]
    }
  }
}
```

**使用场景**:
- 验证受众是否匹配目标人群
- 分析性别/年龄/地域分布
- 了解兴趣偏好
