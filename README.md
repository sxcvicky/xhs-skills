# 小红书数据分析 Skill

> **版本**: v2.0.0 | **验证日期**: 2026-04-13 | 20/20 接口全部可用

通过数据驱动的方法，帮助用户制定小红书内容策略、分析竞品、洞察受众。

## 这个 Skill 能做什么

✅ **内容策略分析**: 找到热门话题中的差异化机会  
✅ **博主合作评估**: 基于互动率和受众匹配度推荐博主  
✅ **竞品分析**: 对比竞品内容策略，找出机会点  
✅ **数据解读**: 将原始数据转化为可执行建议  

## API 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: 所有接口需要 `X-API-Key` Header
- **路径前缀**: 所有业务接口必须带 `/api` 前缀（例如 `/api/v1/discovery/...`）

## 安装方式

```bash
# 复制 Skill 到项目目录
cp -r xhs-data-api <your-project>/.qoder/skills/
```

## 配置

在环境变量中配置 API Key：

```bash
XHS_API_KEY=your-api-key-here
```

## 验证安装

```bash
# 健康检查（无需 API Key）
curl https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/health
```

## 使用示例

### 查询热门关键词

```bash
curl -X POST "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/v1/discovery/keywords/hot" \
  -H "Content-Type: application/json" \
  -H "X-API-Key: $XHS_API_KEY" \
  -d '{"category": "美妆个护", "limit": 10}'
```

### 查询市场供需分析

```bash
curl -X POST "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/v1/benchmark/market-opportunity" \
  -H "Content-Type: application/json" \
  -H "X-API-Key: $XHS_API_KEY" \
  -d '{"category": "手机数码", "timeRange": "30"}'
```

### 查询博主详情

```bash
curl "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/v1/audience/authors/皮肤科辉哥" \
  -H "X-API-Key: $XHS_API_KEY"
```

## 目录结构

```
xhs-data-api/
├── SKILL.md                         # Skill 主文件（思维框架 + 接口总览）
├── README.md                        # 本文件
└── references/
    ├── data-discovery.md            # Discovery 层：5 个发现类接口
    ├── data-analysis.md             # Analysis 层：5 个分析类接口
    ├── data-audience.md             # Audience 层：2 个受众类接口
    ├── data-benchmark.md            # Benchmark 层：4 个对标类接口
    └── data-detail.md               # Detail 层：4 个详情类接口
```

## 接口一览（20 个）

| 层级 | 接口 | 方法 | 路径 |
|------|------|------|------|
| Discovery | 热门关键词 | POST | `/api/v1/discovery/keywords/hot` |
| Discovery | 飙升关键词 | POST | `/api/v1/discovery/keywords/rising` |
| Discovery | 热门话题 | POST | `/api/v1/discovery/topics/hot` |
| Discovery | 爆款笔记 | POST | `/api/v1/discovery/notes/viral` |
| Discovery | 涨粉博主 | POST | `/api/v1/discovery/authors/rising` |
| Analysis | 内容聚合分析 | POST | `/api/v1/content/analysis` |
| Analysis | 内容搜索 | POST | `/api/v1/content/search` |
| Analysis | 内容趋势 | POST | `/api/v1/content/trends` |
| Analysis | 属性卖点概览 | POST | `/api/v1/content/attributes` |
| Analysis | 属性关键词明细 | POST | `/api/v1/content/attribute-keywords` |
| Audience | 活跃博主 | POST | `/api/v1/audience/active` |
| Audience | 受众画像 | POST | `/api/v1/audience/profile` |
| Benchmark | 关键词排名竞赛 | POST | `/api/v1/benchmark/keyword-race` |
| Benchmark | 市场供需分析 | POST | `/api/v1/benchmark/market-opportunity` |
| Benchmark | 品牌竞争对标 | POST | `/api/v1/benchmark/brand-compete` |
| Benchmark | 品牌/SPU排名 | POST | `/api/v1/benchmark/brand-ranking` |
| Detail | 笔记详情 | GET | `/api/v1/content/notes/:noteId` |
| Detail | 博主详情 | GET | `/api/v1/audience/authors/:authorId` |
| Detail | 博主内容分析 | GET | `/api/v1/audience/authors/:authorId/content` |
| Detail | 博主粉丝画像 | GET | `/api/v1/audience/authors/:authorId/fans` |

## 触发关键词

当用户提到以下关键词时，此 Skill 会被激活：
- 小红书话题 / XHS topics
- 博主分析 / blogger insights
- 笔记数据 / note performance
- 关键词查询 / keyword search
- 内容策略 / content strategy
- 竞品分析 / competitor analysis
- 受众画像 / audience profile
- 市场趋势 / market trends

## 许可证

MIT
