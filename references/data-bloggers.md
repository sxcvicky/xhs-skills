# 博主数据接口

用于博主列表查询、粉丝画像分析、互动数据评估。

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## ⚠️ 参数来源约束（重要）

本 API 中 `keyword` 参数有严格来源限制，禁止用户随意填写：

| 参数 | 必须来源 | 说明 |
|------|----------|------|
| `keyword` | `/api/query/contentcapital/keywords` 返回的 `keywordCode` | 关键词代码 |

**工作流示例**：
```
1. 用户问："分析美妆博主的表现"
2. Agent 先调用 /api/query/contentcapital/keywords?category=美妆&pageSize=10
3. 返回 keywordCode 列表
4. Agent 选择 keywordCode，调用 /api/v1/audience/active
```

---

## 博主查询

### 获取博主列表（飙升榜）

```
POST /api/v1/discovery/authors/rising
```

**参数**:
- `keyword` (string): 搜索关键词（**必填**，必须来自 keywords 接口的 keywordCode）
- `page`, `pageSize`: 分页

**⚠️ 约束**：keyword 必须从 `/api/query/contentcapital/keywords` 返回的 keywordCode 中选择

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
- `keyword` (string): 搜索关键词（**必填**，必须来自 keywords 接口的 keywordCode）
- `authorId` (string): 博主ID

**⚠️ 约束**：keyword 必须从 `/api/query/contentcapital/keywords` 返回的 keywordCode 中选择

**响应**:
```json
{
  "success": true,
  "data": {
    "keyword": "??",
    "totalAuthors": 3,
    "authors": [
      {
        "nickname": "红鲤鱼与绿鲤鱼与驴V",
        "noteCount": 3,
        "totalEngagement": 60012,
        "avgEngageRate": "2000400.00%"
      }
    ]
  }
}
```

**使用场景**:
- 验证受众是否匹配目标人群
- 分析性别/年龄/地域分布
- 了解兴趣偏好
