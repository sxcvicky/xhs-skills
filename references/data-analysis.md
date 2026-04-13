# Analysis 分析层接口

> ✅ **已验证** (2026-04-13) - 5/5 接口全部可用  
> **版本**: v2.0.0

## 接口列表

### 1. 内容聚合分析

```
POST /api/v1/content/analysis
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "keyword": "华为",
  "category": "手机数码",
  "timeRange": "30"
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "keyword": "华为",
    "category": "手机数码",
    "overview": {
      "totalNotes": 11644608334,
      "totalReads": 5776943822745,
      "avgEngagement": 496,
      "engagementRate": "0%"
    },
    "formatDistribution": {
      "image": { "count": 8135023782, "ratio": "71.37%" },
      "video": { "count": 3263507413, "ratio": "28.63%" }
    },
    "themeDistribution": [
      { "theme": "单品推荐", "count": 38151472, "avgEngagement": 0 }
    ],
    "topTags": [
      { "tag": "小马糕", "count": 124975519 }
    ]
  }
}
```

---

### 2. 内容搜索

```
POST /api/v1/content/search
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "keyword": "护肤",
  "sortBy": "hot",
  "page": 1,
  "pageSize": 20
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "total": 12500,
    "page": 1,
    "pageSize": 20,
    "notes": [
      {
        "noteId": "698af478000000001d026bb4",
        "title": "皮肤科医生才知道的 正确护肤顺序‼️",
        "author": "皮肤科辉哥",
        "type": "unknown",
        "engagement": 222373,
        "reads": 7123881,
        "publishTime": "2026-02-10T00:00:00"
      }
    ]
  }
}
```

---

### 3. 内容趋势分析

```
POST /api/v1/content/trends
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "keyword": "华为",
  "timeRange": "30",
  "granularity": "day"
}
```

**注意**: 参数名是 `granularity` 不是 `interval`

---

### 4. 属性卖点概览

```
POST /api/v1/content/attributes
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "category": "美妆个护",
  "timeRange": "30"
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "category": "美妆个护",
    "timeRange": "30",
    "attributes": [
      {
        "attributeCategory": "整体",
        "noteNum": 16019506,
        "ratio": "100.00%",
        "top3Word": "梦核紫,胶片绿,938元",
        "readNum": null,
        "engageNum": null
      }
    ],
    "dataNote": "readNum/engageNum 数据来自灵犀API，该接口不返回这些指标"
  }
}
```

**字段说明**:
- `attributeCategory`: 属性分类（整体/人群/买点/场景/品类产品）
- `noteNum`: 笔记数
- `ratio`: 占比
- `top3Word`: TOP3关键词
- `readNum`: 阅读数（⚠️ 部分类目可能为null）
- `engageNum`: 互动数（⚠️ 部分类目可能为null）

---

### 5. 属性关键词明细

```
POST /api/v1/content/attribute-keywords
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "category": "美妆个护",
  "timeRange": "30",
  "attributeCategory": "人群",
  "dimensionIndex": "noteNum",
  "limit": 50
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "attributeCategory": "人群",
    "keywords": [
      { "keyword": "学生党", "metricValue": 3456, "ranking": 1, "growthRate": "12.5%" }
    ]
  }
}
```
