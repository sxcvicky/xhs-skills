# 竞品分析数据接口

用于竞品博主对比、内容策略分析、受众重叠度分析。

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## ⚠️ 参数来源约束（重要）

本 API 中 `keyword` 和 `category` 参数有严格来源限制，禁止用户随意填写：

| 参数 | 必须来源 | 说明 |
|------|----------|------|
| `keyword` | `/api/query/contentcapital/keywords` 返回的 `keywordCode` | 关键词代码 |
| `category` | 话题/关键词接口返回的 `category` | 类目名称如"彩妆"、"母婴" |
| `brandKeyword` | 品牌名称（可自由填写） | 竞品品牌名称 |

**工作流示例**：
```
1. 用户问："分析雅诗兰黛在美妆行业的竞争情况"
2. Agent 先调用 /api/query/contentcapital/keywords?category=美妆&pageSize=10
3. 返回 keywordCode 列表
4. Agent 选择 keywordCode（如"保湿补水"），调用 /api/v1/benchmark/brand-compete
   - brandKeyword: "雅诗兰黛"
   - category: "美妆"
   - keywordCode: "保湿补水"
```

---

## 竞品对比

### 品牌竞争对标

```
POST /api/v1/benchmark/brand-compete
```

**参数**:
- `brandKeyword` (string): 品牌关键词（**必填**，自由填写竞品品牌名）
- `keyword` (string): 关键词（**必填**，必须来自 keywords 接口的 keywordCode）
- `category` (string): 类目（**必填**，必须来自话题/关键词接口的 category）
- `timeRange` (string): 时间范围（必填，"7" 或 "30"）

**⚠️ 约束**：
- keyword 必须从 `/api/query/contentcapital/keywords` 返回的 keywordCode 中选择
- category 必须从话题/关键词接口返回的 category 中选择

**响应**:
```json
{
  "success": true,
  "data": {
    "category": "美妆",
    "timeRange": "30",
    "brands": [...]
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
- `keyword` (string): 关键词（**必填**，必须来自 keywords 接口的 keywordCode）
- `category` (string): 类目（**必填**，必须来自话题/关键词接口的 category）
- `timeRange` (string): 时间范围（"7" 或 "30"）

**⚠️ 约束**：
- keyword 必须从 `/api/query/contentcapital/keywords` 返回的 keywordCode 中选择
- category 必须从话题/关键词接口返回的 category 中选择

**响应**:
```json
{
  "success": true,
  "data": {
    "category": "美妆",
    "timeRange": "30",
    "data": {
      "searchNum": 0,
      "noteNum": 0,
      "brandNum": 0,
      "medianSearchNum": 0,
      "medianNoteNum": 0
    }
  }
}
```

**使用场景**:
- 分析市场供需平衡
- 找出蓝海市场机会
