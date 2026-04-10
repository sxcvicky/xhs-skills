# 小红书数据 API 完整接口文档

**基础 URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`  
**认证方式**: 请求头 `X-API-Key: YOUR_API_KEY`  
**响应格式**: `{code: number, message?: string, data: any}`

---

## 🔍 一、数据查询类

### 1.1 话题列表

```
GET /api/query/contentcapital/topics
```

**查询参数**:
- `page` (number): 页码，默认 1
- `pageSize` (number): 每页数量，默认 20
- `keyword` (string): 搜索关键词

**响应**:
```json
{
  "code": 200,
  "data": {
    "list": [
      {
        "id": 1,
        "word": "护肤",
        "viewCount": 58700000000,
        "noteCount": 12500000,
        "relatedWords": ["护肤品", "护肤routine"]
      }
    ],
    "pagination": {
      "page": 1,
      "pageSize": 20,
      "total": 4217,
      "totalPages": 211
    }
  }
}
```

### 1.2 关键词列表

```
GET /api/query/contentcapital/keywords
```

**查询参数**: 同上

**响应**:
```json
{
  "code": 200,
  "data": {
    "list": [
      {
        "id": 1,
        "word": "敏感肌",
        "category": "肤质",
        "searchCount": 8500000
      }
    ],
    "pagination": { ... }
  }
}
```

### 1.3 笔记列表

```
GET /api/query/contentcapital/notes
```

**查询参数**:
- `page`, `pageSize`: 分页
- `topicId` (number): 话题ID
- `keyword` (string): 搜索关键词

**响应**:
```json
{
  "code": 200,
  "data": {
    "list": [
      {
        "id": 1,
        "noteId": "64f09653000000000200d13f",
        "title": "敏感肌护肤指南",
        "likeCount": 12500,
        "collectCount": 8900,
        "commentCount": 456
      }
    ],
    "pagination": { ... }
  }
}
```

### 1.4 博主列表

```
GET /api/query/contentcapital/bloggers
```

**查询参数**:
- `page`, `pageSize`: 分页
- `keyword` (string): 搜索关键词
- `fansMin` (number): 最小粉丝数

**响应**:
```json
{
  "code": 200,
  "data": {
    "list": [
      {
        "id": 1,
        "userId": "5a2c3e4f5b6c7d8e9f0a1b2c",
        "nickname": "护肤小达人",
        "fansCount": 1250000,
        "noteCount": 356,
        "likeAndCollectCount": 8900000
      }
    ],
    "pagination": { ... }
  }
}
```

---

## 📊 二、业务分析类

### 2.1 内容策略分析

```
GET /api/analysis/content-strategy
```

**查询参数**:
- `topicId` (number): 话题ID
- `timeRange` (string): 时间范围 (7d/30d/90d)

**响应**:
```json
{
  "code": 200,
  "data": {
    "topicAnalysis": {
      "trend": "rising",
      "competition": "medium",
      "opportunity": 85
    },
    "contentRecommendations": [
      {
        "type": "图文",
        "angle": "敏感肌秋冬护肤routine",
        "reason": "搜索量上升45%"
      }
    ]
  }
}
```

### 2.2 受众画像分析

```
GET /api/audience/profile
```

**查询参数**:
- `topicId` (number): 话题ID
- `bloggerId` (number): 博主ID

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

### 2.3 竞品对比分析

```
GET /api/benchmark/compare
```

**查询参数**:
- `bloggerIds` (string): 博主ID列表，逗号分隔

**响应**:
```json
{
  "code": 200,
  "data": {
    "comparison": [
      {
        "bloggerId": "5a2c3e4f5b6c7d8e9f0a1b2c",
        "nickname": "博主A",
        "metrics": {
          "avgLikes": 12500,
          "avgCollects": 8900,
          "engagementRate": 8.5
        }
      }
    ]
  }
}
```

---

## 🔧 三、辅助接口

### 3.1 健康检查

```
GET /api/health
```

**响应**:
```json
{
  "code": 200,
  "data": {
    "status": "ok",
    "uptime": 86400,
    "database": "connected"
  }
}
```

### 3.2 获取 API 文档

```
GET /api/docs
```

返回 Swagger UI 页面

---

## ⚠️ 错误码说明

| 错误码 | HTTP 状态码 | 说明 | 处理方式 |
|--------|-------------|------|----------|
| 401 | 401 | API Key 无效或缺失 | 检查 X-API-Key 配置 |
| 429 | 429 | 请求过于频繁 | 等待 1-2 秒后重试 |
| 500 | 500 | 服务器内部错误 | 记录错误，稍后重试 |
| 404 | 404 | 接口不存在 | 检查接口路径 |
| 400 | 400 | 请求参数错误 | 检查参数格式和类型 |

---

## 💡 最佳实践

1. **分页查询**: 大数据量时使用 `page` 和 `pageSize` 分页
2. **频率控制**: 请求间隔 100-300ms，避免触发限流
3. **缓存策略**: 种子池统计等静态数据可缓存 5-10 分钟
4. **错误重试**: 500 错误最多重试 3 次，间隔递增
5. **批量操作**: 爬虫触发时设置合理的 `maxTasks` (建议 5-10)

---

## 📝 更新日志

- **v1.0.0** (2026-04-10): 初始版本，包含核心查询和分析接口
