# 内容策略数据接口

用于分析话题热度、关键词趋势、笔记爆款特征。

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## 话题查询

### 获取话题列表

```
GET /api/query/contentcapital/topics
```

**参数**:
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

**使用场景**:
- 了解市场大盘热度
- 找到高热度低竞争话题
- 分析相关词扩展内容角度

---

## 关键词查询

### 获取关键词列表

```
GET /api/query/contentcapital/keywords
```

**参数**: 同上

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

**使用场景**:
- 发现细分需求
- 分析搜索趋势
- 定位长尾机会

---

## 笔记查询

### 获取笔记列表

```
GET /api/query/contentcapital/notes
```

**参数**:
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

**使用场景**:
- 分析爆款笔记特征
- 学习内容角度和标题
- 计算互动率（收藏/点赞比）

---

## 内容策略分析

### 综合内容策略分析

```
GET /api/analysis/content-strategy
```

**参数**:
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

**使用场景**:
- 快速获得内容策略建议
- 了解话题趋势和竞争度
- 发现内容机会点
