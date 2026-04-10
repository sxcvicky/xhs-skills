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

## 爆款笔记

### 获取爆款笔记列表

```
POST /api/v1/discovery/notes/viral
```

**参数**: 无需参数，返回近期爆款笔记

**响应**:
```json
{
  "success": true,
  "data": {
    "notes": [
      {
        "noteId": "6911b1a50000000005013042",
        "title": "医生，我有点舒服了，我想回家",
        "author": "皮肤科主任李茜",
        "likes": 1538730,
        "reads": 9153351,
        "publishTime": "2025-11-10",
        "engagement": 1538730
      }
    ]
  }
}
```

**使用场景**:
- 分析爆款笔记特征
- 学习内容角度和标题
- 了解用户偏好

---

## 内容策略分析

### 品牌内容分析

```
POST /api/v1/content/analysis
```

**参数**:
- `keyword` (string): 关键词（必填）
- `category` (string): 类目
- `timeRange` (number): 时间范围（天数，默认30）

**响应**:
```json
{
  "success": true,
  "data": {
    "overview": { ... },
    "propagationTypes": { ... },
    "hotTopics": { ... },
    "hotWords": [ ... ]
  }
}
```

**使用场景**:
- 快速获得内容策略建议
- 了解话题趋势和竞争度
- 发现内容机会点

