# 竞品分析数据接口

用于竞品博主对比、内容策略分析、受众重叠度分析。

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## 竞品对比

### 品牌竞争对标

```
POST /api/v1/benchmark/brand-compete
```

**参数**:
- `brandKeyword` (string): 品牌关键词（必填）
- `category` (string): 类目（必填，如"美妆"）
- `timeRange` (string): 时间范围（必填，"7" 或 "30"）

**响应**:
```json
{
  "success": true,
  "data": {
    "brandAnalysis": {
      "trend": "rising",
      "competition": "medium",
      "opportunity": 85
    },
    "recommendations": [
      {
        "type": "内容角度",
        "angle": "敏感肌护肤",
        "reason": "搜索量上升45%"
      }
    ]
  }
}
```

**使用场景**:
- 分析品牌竞争格局
- 找出竞争优势和劣势
- 制定差异化策略

---

## 市场分析

### 市场供需分析

```
POST /api/v1/benchmark/market-opportunity
```

**参数**:
- `keyword` (string): 关键词（必填）
- `category` (string): 类目（必填，如"美妆"）
- `timeRange` (string): 时间范围（"7" 或 "30"）

**响应**:
```json
{
  "success": true,
  "data": {
    "data": {
      "searchNum": 0,
      "noteNum": 0,
      "brandNum": 0
    }
  }
}
```

**使用场景**:
- 分析市场供需平衡
- 找出蓝海市场机会
