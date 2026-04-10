---
name: xhs-data-api
description: >-
  Query Xiaohongshu (Little Red Book) data via HTTP API including topics, bloggers, 
  notes, keywords, and seed pool stats. Use when user asks for XHS topic analysis, 
  blogger insights, note performance, competitor analysis, content strategy, or 
  crawler status. All API calls require X-API-Key header for authentication.
version: 1.0.0
user-invocable: true
metadata: {"openclaw":{"emoji":"📊","requires":{"env":["XHS_API_KEY"]},"primaryEnv":"XHS_API_KEY"}}
---

# 小红书数据 API 使用指南

## 角色定位

你是小红书数据查询专家，通过 HTTP API 获取小红书平台的话题、博主、笔记、关键词等数据。

## 核心规则

1. **认证**: 所有接口必须在请求头添加 `X-API-Key`
2. **基础 URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
3. **响应格式**: 所有接口返回 `{code, message, data}` 结构
4. **分页**: 列表接口支持 `page` 和 `pageSize` 参数
5. **频率限制**: 建议请求间隔 100-300ms

## 快速参考

### 爬虫触发
| 接口 | 方法 | 说明 |
|------|------|------|
| `/api/crawler/mkt/run` | POST | 触发MKT爬虫(Realtime+PGY) |
| `/api/crawler/mkt/seed-stats` | GET | 查询种子池统计 |
| `/api/crawler/status/:taskId` | GET | 查询任务状态 |

### 数据查询
| 接口 | 方法 | 说明 |
|------|------|------|
| `/api/query/contentcapital/topics` | GET | 话题列表 |
| `/api/query/contentcapital/keywords` | GET | 关键词列表 |
| `/api/query/contentcapital/notes` | GET | 笔记列表 |
| `/api/query/contentcapital/bloggers` | GET | 博主列表 |

### 业务分析
| 接口 | 方法 | 说明 |
|------|------|------|
| `/api/analysis/content-strategy` | GET | 内容策略分析 |
| `/api/audience/profile` | GET | 受众画像分析 |
| `/api/benchmark/compare` | GET | 竞品对比分析 |

## 调用示例

### 1. 查询种子池统计

```bash
curl -X GET "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/crawler/mkt/seed-stats" \
  -H "X-API-Key: YOUR_API_KEY"
```

**响应示例**:
```json
{
  "code": 200,
  "data": {
    "realtime": { "words": 4217, "themes": 15 },
    "pgy": { "words": 382 }
  }
}
```

### 2. 查询话题列表

```bash
curl -X GET "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/query/contentcapital/topics?page=1&pageSize=20" \
  -H "X-API-Key: YOUR_API_KEY"
```

**响应示例**:
```json
{
  "code": 200,
  "data": {
    "list": [
      {
        "id": 1,
        "word": "护肤",
        "viewCount": 58700000000,
        "noteCount": 12500000
      }
    ],
    "pagination": {
      "page": 1,
      "pageSize": 20,
      "total": 4217,
      "totalPages": 211
    }
  }
}
```

### 3. 触发 MKT 爬虫

```bash
curl -X POST "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/crawler/mkt/run" \
  -H "X-API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"source": "realtime", "maxTasks": 5}'
```

## 错误处理

| 错误码 | 说明 | 处理方式 |
|--------|------|----------|
| 401 | API Key 无效 | 检查 X-API-Key 配置 |
| 429 | 请求过于频繁 | 等待后重试 |
| 500 | 服务器错误 | 记录错误，稍后重试 |

## 详细文档

完整接口文档（20+ 接口）请参考：
- [完整接口文档](references/api-endpoints.md)

## 注意事项

- ⚠️ 不要在 Skill 中硬编码 API Key，使用环境变量
- ⚠️ 批量查询时注意分页和频率限制
- ⚠️ 爬虫触发后需要轮询状态接口获取进度
- ✅ 优先使用查询接口，避免重复触发爬虫
- ✅ 大数据量查询时分页获取
