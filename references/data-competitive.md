# 竞品分析数据接口

用于竞品博主对比、内容策略分析、受众重叠度分析。

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## 竞品对比

### 竞品对比分析

```
GET /api/benchmark/compare
```

**参数**:
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

**使用场景**:
- 对比多个竞品博主表现
- 分析互动率差异
- 找出竞争优势和劣势
