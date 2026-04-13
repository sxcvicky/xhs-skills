# Discovery 发现层接口

> ✅ **已验证** (2026-04-13) - 5/5 接口全部可用  
> **版本**: v2.0.0

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## 接口列表

### 1. 热门话题发现

```
POST /api/v1/discovery/topics/hot
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "category": "美妆个护",
  "timeRange": "30",
  "sortBy": "hotIndex",
  "limit": 20
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "topics": [
      {
        "topicId": "6953794d0000000025037b30",
        "topicName": "王者小马糕",
        "hotIndex": 95000,
        "noteCount": 3500,
        "participateCount": 2800
      }
    ]
  },
  "meta": {
    "dataSource": "lingxi_topic",
    "crawledAt": "2026-04-13T02:10:14.000Z"
  }
}
```

**注意**: 部分类目可能没有话题数据，建议先用 keywords 接口

---

### 2. 热门关键词发现

```
POST /api/v1/discovery/keywords/hot
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "category": "美妆个护",
  "keyword": "护肤",
  "timeRange": "30",
  "limit": 20
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "keywords": [
      {
        "keyword": "手机壳",
        "searchCount": 15544382,
        "growth": 0,
        "trend": "stable",
        "ranking": 1
      }
    ]
  }
}
```

**字段说明**:
- `keyword`: 关键词名称
- `searchCount`: 搜索量
- `growth`: 增长率百分比
- `trend`: 趋势（stable/up/down）
- `ranking`: 排名

**可选参数**:
- `keyword`: 关键词过滤（模糊匹配）

---

### 3. 飙升关键词发现

```
POST /api/v1/discovery/keywords/rising
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "category": "美妆个护",
  "growthMin": 50,
  "timeRange": "30",
  "limit": 20
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "keywords": [
      {
        "keyword": "华为",
        "searchCount": 6131918,
        "growth": 180,
        "trend": "up",
        "ranking": 1
      }
    ]
  }
}
```

---

### 4. 爆款笔记发现

```
POST /api/v1/discovery/notes/viral
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "category": "美妆个护",
  "timeRange": "30",
  "minEngagement": 10000,
  "limit": 20
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "notes": [
      {
        "noteId": "69b385a60000000015039b1d",
        "title": "当我用彩色修容挑战随机色化妆？！杀疯了！",
        "author": "弛仟_Chyqan",
        "engagement": 854069,
        "reads": 6200704,
        "growthRate": 100,
        "publishTime": "2026-03-13T00:00:00"
      }
    ]
  }
}
```

**字段说明**:
- `noteId`: 笔记ID（可用于详情接口）
- `title`: 笔记标题
- `author`: 作者昵称
- `engagement`: 互动量（点赞+评论+分享+收藏）
- `reads`: 阅读数
- `publishTime`: 发布时间

---

### 5. 涨粉博主发现

```
POST /api/v1/discovery/authors/rising
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "category": "美妆个护",
  "timeRange": "30",
  "minGrowth": 1000,
  "limit": 20
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "authors": [
      {
        "authorId": "xxx",
        "nickname": "博主昵称",
        "fansCount": 580000,
        "fansGrowth": 80000,
        "growthRate": 16,
        "category": "美妆"
      }
    ]
  }
}
```
