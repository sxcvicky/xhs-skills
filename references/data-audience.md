# Audience 受众层接口

> ✅ **已验证** (2026-04-13) - 2/2 接口全部可用  
> **版本**: v2.0.0

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`
- **所有接口使用 POST 方法**

---

## 接口列表

### 1. 活跃博主

```
POST /api/v1/audience/active
```

**状态**: ✅ 可用

**职责**: 查询某关键词下近期活跃的博主列表，含互动率数据

**请求体**:
```json
{
  "keyword": "护肤",
  "timeRange": "30",
  "sortBy": "noteCount",
  "limit": 10
}
```

**参数说明**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `keyword` | string | 是 | 关键词（用户输入即可） |
| `timeRange` | string | 否 | "7" 或 "30" 天，默认 "30" |
| `sortBy` | string | 否 | noteCount(发文数) / fansCount(粉丝数)，默认 noteCount |
| `limit` | number | 否 | 返回条数，默认 20 |

**实测响应**:
```json
{
  "success": true,
  "data": {
    "keyword": "护肤",
    "totalAuthors": 0,
    "authors": [
      {
        "nickname": "珍珍很懒科普",
        "noteCount": 1,
        "totalEngagement": "3677",
        "avgEngageRate": "6.50%",
        "pgyProfile": null
      }
    ]
  },
  "meta": {
    "dataSource": "lingxi",
    "crawledAt": "2026-04-13T02:10:14.000Z"
  }
}
```

**字段说明**:
- `nickname`: 博主昵称
- `noteCount`: 发文数
- `totalEngagement`: 总互动量
- `avgEngageRate`: 平均互动率
- `pgyProfile`: 蒲公英平台数据（为 null 表示该博主未被采集）

**使用场景**:
- 找到某关键词下最活跃的博主
- 评估博主互动表现
- 筛选潜在合作博主

---

### 2. 受众画像

```
POST /api/v1/audience/profile
```

**状态**: ✅ 可用

**职责**: 查询某关键词对应的受众人群画像（性别、年龄、地域、兴趣等）

**请求体**:
```json
{
  "keyword": "华为",
  "timeRange": "30"
}
```

**参数说明**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `keyword` | string | 是 | 关键词 |
| `timeRange` | string | 否 | "7" 或 "30" 天，默认 "30" |

**响应格式**:
```json
{
  "success": true,
  "data": {
    "keyword": "华为",
    "demographics": {
      "gender": { "female": 65.0, "male": 35.0 },
      "age": { "18-24": 25.0, "25-34": 45.0, "35-44": 20.0 }
    },
    "interests": ["数码", "手机", "科技"],
    "location": {
      "topProvinces": ["广东", "北京", "浙江"]
    }
  },
  "meta": {
    "dataSource": "lingxi"
  }
}
```

**字段说明**:
- `demographics.gender`: 性别分布
- `demographics.age`: 年龄段分布
- `interests`: 兴趣标签列表
- `location.topProvinces`: TOP 省份

**注意**: 部分类目数据可能返回空数组，这是正常行为

**使用场景**:
- 了解目标关键词的受众人群特征
- 验证产品与受众匹配度
- 制定内容定向策略

---

## 注意事项

1. 博主详情（博主基本信息、内容分析、粉丝画像）请查看 `references/data-detail.md`
2. 受众画像目前以关键词维度查询，不支持直接按博主查询
