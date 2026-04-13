# 内容策略数据接口

用于分析话题热度、关键词趋势、笔记爆款特征。

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## ⚠️ LLM 参数匹配指引

本 API 中 `keyword` 和 `category` 参数有推荐来源，但 LLM 需要做智能匹配：

| 参数 | 推荐来源 | 说明 |
|------|----------|------|
| `keyword` | `/api/query/contentcapital/keywords` 返回的 `keywordCode` | 关键词代码 |
| `category` | 话题/关键词接口返回的 `category` | 类目名称 |

**LLM 智能匹配规则**：
```
1. 用户问："分析华为手机芯片的市场趋势"
2. LLM 判断：用户想分析"手机芯片"相关趋势
3. LLM 调用 /api/query/contentcapital/keywords 搜索 "手机芯片"
4. 返回 keywordCode 列表中找到 "手机芯片"
5. LLM 使用 "手机芯片" 作为 keyword 参数
```

**匹配策略**：
- 如果用户输入精确匹配到 keywordCode → 直接使用
- 如果用户输入模糊匹配到 keywordCode → 选择最相近的
- 如果没有匹配 → 调用 keywords 接口搜索近似词
- **禁止直接使用用户输入的任意文本作为 keyword**
---

## 话题查询

### 获取话题列表

```
GET /api/query/contentcapital/topics
```

**参数**:
- `page` (number): 页码，默认 1
- `pageSize` (number): 每页数量，默认 20
- `keyword` (string): 搜索话题关键词

**响应**:
```json
{
  "data": [
    {
      "category": "彩妆",
      "taxonomyCode": "xxx",
      "topic": "#口红试色",
      "ranking": 1,
      "noteNum": 99800,
      "readNum": 1820000,
      "engageNum": 65000
    }
  ],
  "total": 3031,
  "page": 1,
  "pageSize": 20
}
```

**可提取字段**：`category` 可作为后续接口的 category 参数

**使用场景**:
- 了解市场大盘热度
- 找到高热度低竞争话题
- 提取 category 用于其他接口

---

## 关键词查询

### 获取关键词列表（keyword 必读）

```
GET /api/query/contentcapital/keywords
```

**参数**:
- `page` (number): 页码，默认 1
- `pageSize` (number): 每页数量，默认 20
- `category` (string): 按类目筛选（可选）

**响应**:
```json
{
  "data": [
    {
      "category": "彩妆",
      "taxonomyCode": "xxx",
      "keywordCode": "保湿补水",
      "keywordName": "保湿补水",
      "value": 7076771570,
      "attributeCodes": ["功效", "肤质", "成分"]
    }
  ],
  "total": 2368,
  "page": 1,
  "pageSize": 20
}
```

**⚠️ 重要**：
- `keywordCode` 字段是后续分析接口的 keyword 参数来源
- `category` 字段可作为其他接口的 category 参数
- 禁止直接使用用户输入的任意文本作为 keyword

**使用场景**:
- 发现细分需求关键词
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
- `keyword` (string): 关键词（**必填**，必须来自 keywords 接口的 keywordCode）
- `category` (string): 类目（可选，必须来自话题/关键词接口的 category）
- `timeRange` (number): 时间范围（天数，默认30）

**约束**：
- LLM 应优先从 keywords 接口获取 keywordCode
- 如果用户输入不在 keywordCode 中，调用 keywords 接口搜索近似词
- **禁止直接使用用户输入的任意文本作为 keyword**

**响应**:
```json
{
  "success": true,
  "data": {
    "keyword": "??",
    "overview": { "totalNotes": 11644608334 },
    "formatDistribution": {
      "image": { "ratio": "71.37%" },
      "video": { "ratio": "28.63%" }
    },
    "themeDistribution": [...],
    "topTags": [...]
  }
}
```

**使用场景**:
- 快速获得内容策略建议
- 了解话题趋势和竞争度
- 发现内容机会点

