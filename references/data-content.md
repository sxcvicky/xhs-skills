# 内容策略数据接口

用于分析话题热度、关键词趋势、笔记爆款特征。

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## ⚠️ 参数来源约束（重要）

本 API 中 `keyword` 和 `category` 参数有严格来源限制，禁止用户随意填写：

| 参数 | 必须来源 | 说明 |
|------|----------|------|
| `keyword` | `/api/query/contentcapital/keywords` 返回的 `keywordCode` | 关键词代码，非任意文本 |
| `category` | 话题/关键词接口返回的 `category` | 类目名称如"彩妆"、"母婴" |

**工作流示例**：
```
1. 用户问："分析美妆行业的护肤趋势"
2. Agent 先调用 /api/query/contentcapital/keywords?category=美妆&pageSize=10
3. 返回 keywordCode 列表：["保湿补水", "祛斑", "敏感肌"...]
4. Agent 选择 "保湿补水"，调用 /api/v1/content/analysis
```

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

**⚠️ 约束**：
- keyword 参数禁止用户随意填写，必须从 `/api/query/contentcapital/keywords` 返回的 keywordCode 中选择
- 如果用户提供的关键词不在 keywordCode 列表中，需先调用关键词接口查询

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

