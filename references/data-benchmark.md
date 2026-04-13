# Benchmark 对标层接口

> ✅ **已验证** (2026-04-13) - 4/4 接口全部可用

## 接口列表

### 1. 关键词排名竞赛

```
POST /api/v1/benchmark/keyword-race
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "keyword": "手机",
  "brands": "华为,小米,苹果",
  "timeRange": "30"
}
```

---
### 2. 市场供需分析

```
POST /api/v1/benchmark/market-opportunity
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "category": "手机数码",
  "timeRange": "30"
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "category": "手机",
    "timeRange": "30",
    "data": {
      "searchNum": 416337202,
      "noteNum": 24442215,
      "brandNum": 1739,
      "medianSearchNum": 288100792,
      "medianNoteNum": 14993784
    }
  }
}
```

**字段说明**:
- `searchNum`: 搜索量
- `noteNum`: 笔记数
- `brandNum`: 品牌数
- `medianSearchNum`: 中位数搜索量
- `medianNoteNum`: 中位数笔记数

---
### 3. 品牌竞争对标

```
POST /api/v1/benchmark/brand-compete
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "category": "手机数码",
  "timeRange": "30"
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "category": "手机",
    "timeRange": "30",
    "brands": [
      {
        "brandName": "iQOO",
        "searchNum": 2236660,
        "readNum": 60845163,
        "noteNum": 53920,
        "meanSearchNum": 10477649.5,
        "meanReadNum": 334800209.5
      }
    ]
  }
}
```

**字段说明**:
- `brandName`: 品牌名称
- `searchNum`: 搜索量
- `readNum`: 阅读量
- `noteNum`: 笔记数
- `meanSearchNum`: 平均搜索量
- `meanReadNum`: 平均阅读量

---
### 4. 品牌/SPU排名

```
POST /api/v1/benchmark/brand-ranking
```

**状态**: ✅ 可用

**请求体**:
```json
{
  "category": "手机数码",
  "timeRange": "30",
  "entityType": "brand",
  "dimensionIndex": "searchNum"
}
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "category": "手机",
    "timeRange": "30",
    "entityType": "brand",
    "dimensionIndex": "searchNum",
    "rankings": [
      {
        "entityName": "OPPO",
        "entityCode": "239959",
        "rankValue": 14892569,
        "ranking": 3,
        "isPeak": false
      }
    ]
  }
}
```

**字段说明**:
- `entityName`: 实体名称（品牌或SPU）
- `entityCode`: 实体编码
- `rankValue`: 排名值（根据dimensionIndex不同含义不同）
- `ranking`: 排名
- `isPeak`: 是否峰值
