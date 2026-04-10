# 竞品分析数据接口

用于竞品博主对比、内容策略分析、受众重叠度分析。

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## ⚠️ LLM 参数匹配指引

本 API 中 `keyword` 和 `category` 参数有推荐来源，LLM 需要做智能匹配：

| 参数 | 推荐来源 | 说明 |
|------|----------|------|
| `keyword` | `/api/query/contentcapital/keywords` 返回的 `keywordCode` | 关键词代码 |
| `category` | 话题/关键词接口返回的 `category` | 类目名称 |
| `brandKeyword` | 品牌名称（可自由填写） | 竞品品牌名称 |

**LLM 智能匹配规则**：
```
1. 用户问："分析雅诗兰黛在美妆行业的竞争情况"
2. LLM 判断：用户想分析"美妆"行业
3. LLM 调用 /api/query/contentcapital/keywords?category=美妆&pageSize=10
4. 返回 keywordCode 列表
5. LLM 选择合适的 keywordCode（如"保湿补水"）
6. LLM 调用 /api/v1/benchmark/brand-compete:
   - brandKeyword: "雅诗兰黛" (用户输入)
   - category: "美妆" (从接口获取)
   - keyword: "保湿补水" (从接口获取)
```

**匹配策略**：
- 如果用户输入精确/模糊匹配到 keywordCode → 直接使用
- 如果没有匹配 → 调用 keywords 接口搜索近似词
- **禁止直接使用用户输入的任意文本作为 keyword**
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

**约束**：
- LLM 应优先从 keywords 接口获取 keywordCode
- 如果用户输入不在 keywordCode 中，调用 keywords 接口搜索近似词
- **禁止直接使用用户输入的任意文本作为 keyword**

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

**约束**：
- LLM 应优先从 keywords 接口获取 keywordCode
- 如果用户输入不在 keywordCode 中，调用 keywords 接口搜索近似词
- **禁止直接使用用户输入的任意文本作为 keyword**

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
