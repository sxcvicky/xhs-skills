# Detail 详情层接口

> ✅ **已验证** (2026-04-13) - 4/4 接口全部可用  
> **版本**: v2.0.0

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`
- **所有接口使用 GET 方法**

---

## 接口列表

### 1. 笔记详情

```
GET /api/v1/content/notes/:noteId
```

**状态**: ✅ 可用

**职责**: 通过笔记 ID 获取单篇笔记的完整数据（曝光、阅读、互动等）

**路径参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `noteId` | string | 是 | 笔记 ID（可从 discovery/notes/viral 或 content/search 接口获取） |

**调用示例**:
```
GET /api/v1/content/notes/69b385a60000000015039b1d
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "noteId": "69b385a60000000015039b1d",
    "title": "当我用彩色修容挑战随机色化妆？！杀疯了！",
    "author": "弛仟_Chyqan",
    "type": "unknown",
    "publishTime": "2026-03-13T00:00:00",
    "exposure": 6667944,
    "reads": 6200704,
    "interactions": 854069,
    "ctr": "1.02%",
    "engageRate": "13.77%",
    "fullViewsRate": "-",
    "posterUrl": "http://ci.xiaohongshu.com/...",
    "linkUrl": "https://www.xiaohongshu.com/discovery/item/...",
    "brand": null
  },
  "meta": {
    "dataSource": "lingxi_core_note",
    "crawledAt": null
  }
}
```

**字段说明**:

| 字段 | 说明 |
|------|------|
| `noteId` | 笔记 ID |
| `title` | 标题 |
| `author` | 作者昵称 |
| `type` | 笔记类型（image/video/unknown） |
| `publishTime` | 发布时间 |
| `exposure` | 曝光量 |
| `reads` | 阅读量 |
| `interactions` | 互动量（点赞+评论+收藏） |
| `ctr` | 点击率 |
| `engageRate` | 互动率 |
| `fullViewsRate` | 完播率（视频笔记，图文为 "-"） |
| `posterUrl` | 封面图 URL |
| `linkUrl` | 笔记原链接 |
| `brand` | 合作品牌（无则为 null） |

---

### 2. 博主详情

```
GET /api/v1/audience/authors/:authorId
```

**状态**: ✅ 可用

**职责**: 获取博主基本信息，支持两种查询方式

**路径参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `authorId` | string | 是 | 博主 ID 或 博主昵称（支持降级查询） |

**查询方式说明**:
1. **使用 PGY user_id**（优先）：返回完整商业画像数据，dataSource = `"pgy"`
2. **使用博主昵称**（降级）：自动降级到灵犀聚合数据，dataSource = `"lingxi"`

**调用示例**:
```
GET /api/v1/audience/authors/皮肤科辉哥
```

**实测响应（使用昵称降级查询）**:
```json
{
  "success": true,
  "data": {
    "authorId": "皮肤科辉哥",
    "nickname": "皮肤科辉哥",
    "fansCount": 0,
    "avgReadNum": 7123881,
    "avgEngageNum": 222373,
    "dataSource": "lingxi"
  }
}
```

**字段说明**:

| 字段 | 说明 |
|------|------|
| `nickname` | 博主昵称 |
| `fansCount` | 粉丝数（降级时可能为 0） |
| `avgReadNum` | 平均阅读数 |
| `avgEngageNum` | 平均互动数 |
| `dataSource` | 数据来源：pgy（完整）/ lingxi（降级） |

---

### 3. 博主内容分析

```
GET /api/v1/audience/authors/:authorId/content
```

**状态**: ✅ 可用

**职责**: 获取博主发布内容的统计分析（笔记总数、曝光、互动、热门笔记列表）

**路径参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `authorId` | string | 是 | 博主 ID 或昵称（同上支持降级） |

**调用示例**:
```
GET /api/v1/audience/authors/皮肤科辉哥/content
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "authorId": "皮肤科辉哥",
    "nickname": "皮肤科辉哥",
    "totalNotes": 320,
    "totalExposure": 15000000,
    "totalReads": 8500000,
    "totalInteractions": 680000,
    "contentTypes": [
      { "type": 1, "count": 120, "avgEngagement": 1500 },
      { "type": 2, "count": 200, "avgEngagement": 2500 }
    ],
    "topNotes": [
      {
        "noteId": "n_123",
        "title": "华为Mate70深度测评",
        "engageNum": 120000,
        "createTime": "2026-03-20"
      }
    ]
  }
}
```

**字段说明**:

| 字段 | 说明 |
|------|------|
| `totalNotes` | 总发文数 |
| `totalExposure` | 总曝光量 |
| `totalReads` | 总阅读量 |
| `totalInteractions` | 总互动量 |
| `contentTypes` | 内容类型分布（type: 1=图文, 2=视频） |
| `topNotes` | 热门笔记列表 |

**使用场景**:
- 评估博主内容产出能力
- 分析博主内容风格偏好
- 找到博主代表作

---

### 4. 博主粉丝画像

```
GET /api/v1/audience/authors/:authorId/fans
```

**状态**: ✅ 可用

**职责**: 获取博主粉丝的人群画像（年龄、性别、地域、兴趣、设备等）

**路径参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `authorId` | string | 是 | 博主 ID 或昵称 |

**调用示例**:
```
GET /api/v1/audience/authors/皮肤科辉哥/fans
```

**实测响应**:
```json
{
  "success": true,
  "data": {
    "fansCount": 580000,
    "ageDistribution": [
      { "age": "18-24", "ratio": "25%" },
      { "age": "25-34", "ratio": "45%" },
      { "age": "35-44", "ratio": "20%" }
    ],
    "genderDistribution": [
      { "gender": "女", "ratio": "65%" },
      { "gender": "男", "ratio": "35%" }
    ],
    "interestTopics": [
      { "interest": "数码", "ratio": "45%" },
      { "interest": "科技", "ratio": "30%" }
    ],
    "deviceTypes": [
      { "device": "iPhone", "ratio": "60%" },
      { "device": "Android", "ratio": "40%" }
    ],
    "locationDistribution": [
      { "province": "广东", "ratio": "20%" },
      { "province": "北京", "ratio": "15%" }
    ],
    "message": "部分数据可能为null"
  }
}
```

**字段说明**:

| 字段 | 说明 |
|------|------|
| `fansCount` | 粉丝总数 |
| `ageDistribution` | 年龄分布 |
| `genderDistribution` | 性别分布 |
| `interestTopics` | 兴趣偏好 |
| `deviceTypes` | 设备类型分布 |
| `locationDistribution` | 地域分布（省级） |

**注意**: 部分字段可能为 null，这是正常行为（数据来源限制）

**使用场景**:
- 验证博主粉丝与目标人群是否匹配
- 了解粉丝消费力和设备偏好
- 对比多位博主的粉丝画像差异

---

## 注意事项

1. **ID 优先于昵称**: 优先使用 PGY user_id，能获取更完整的数据
2. **降级机制**: 昵称查询会自动降级到灵犀数据，部分字段（如 fansCount）可能为 0
3. **noteId 来源**: 从 `POST /api/v1/discovery/notes/viral` 或 `POST /api/v1/content/search` 接口获取
