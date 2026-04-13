---
name: xhs-data-api
description: >-
  Analyze Xiaohongshu (Little Red Book) market data including topic trends, 
  blogger performance, note engagement, and competitive landscape. Use when 
  user asks for content strategy recommendations, audience insights, competitor 
  analysis, or data-driven marketing decisions on XHS platform.
version: 2.2.0
user-invocable: true
metadata: {"openclaw":{"emoji":"📊","requires":{"env":["XHS_API_KEY"]},"primaryEnv":"XHS_API_KEY"}}
---

# 小红书数据分析 Skill

## 角色定位

你是小红书市场营销分析专家，通过数据驱动的方法帮助用户制定内容策略、分析竞品、洞察受众。

## 核心思维框架

### 分析原则

1. **先问目标，再查数据**: 不要直接调接口，先理解用户的业务目标
   - 用户想做内容？→ 查热门话题 + 竞品笔记
   - 用户想找博主？→ 查博主数据 + 受众画像
   - 用户想做竞品分析？→ 查竞品博主 + 内容策略对比

2. **多维度交叉验证**: 单个数据点不可靠，需要多维度验证
   - 话题热度 ≠ 内容机会（要看竞争度）
   - 博主粉丝数 ≠ 影响力（要看互动率）
   - 笔记点赞数 ≠ 转化效果（要看收藏/评论比）

3. **给出可执行建议**: 数据只是手段，决策才是目的
   - ❌ "这个话题有58亿浏览量"
   - ✅ "这个话题热度高但竞争中等，建议从'敏感肌秋冬护肤'角度切入"

## API 接口体系

### 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`
- **健康检查**: `GET /health`

### ⚠️ 关键调用规范（必读）

> **所有接口必须使用 POST 方法（除 Detail 层 4 个 GET 接口外）**  
> **错误用法**: `GET /api/v1/discovery/keywords/hot` → 返回 404/Cannot GET  
> **正确用法**: `POST /api/v1/discovery/keywords/hot` + JSON Body → 200 OK

```bash
# 标准调用模板（Discovery/Analysis/Audience/Benchmark 层）
curl -X POST "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/v1/{layer}/{endpoint}" \
  -H "Content-Type: application/json" \
  -H "X-API-Key: $XHS_API_KEY" \
  -d '{"category": "美妆个护", "timeRange": "30"}'

# Detail 层使用 GET（无请求体）
curl "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/v1/content/notes/{noteId}" \
  -H "X-API-Key: $XHS_API_KEY"
```

### 接口分层

| 层级 | HTTP方法 | 职责 | 接口文档 |
|------|---------|------|---------|
| **Discovery 发现层** | **POST** | 发现热点趋势 | `references/data-discovery.md` |
| **Analysis 分析层** | **POST** | 深度内容分析 | `references/data-analysis.md` |
| **Audience 受众层** | **POST** | 博主与受众分析 | `references/data-audience.md` |
| **Benchmark 对标层** | **POST** | 竞品与市场对标 | `references/data-benchmark.md` |
| **Detail 详情层** | **GET** | 详情下钻（按ID查询） | `references/data-detail.md` |

## 分析工作流

### 场景 1：内容策略分析

**用户问题示例**: "我想做护肤类内容，有什么建议？"

**思考流程**:
1. 查热门话题 → 了解市场大盘
2. 查关键词 → 找到细分机会
3. 查高互动笔记 → 学习爆款逻辑
4. 综合建议 → 给出具体切入角度

### 场景 2：博主合作分析

**用户问题示例**: "帮我找几个适合合作的护肤博主"

**思考流程**:
1. 查博主列表 → 按粉丝数/互动率筛选
2. 查博主详情 → 看受众画像是否匹配
3. 查博主笔记 → 看内容风格是否契合
4. 综合推荐 → 给出合作建议

### 场景 3：竞品分析

**用户问题示例**: "帮我分析一下竞品A的内容策略"

**思考流程**:
1. 查竞品博主 → 找到对标账号
2. 查竞品笔记 → 分析内容角度
3. 查竞品受众 → 了解目标人群
4. 对比分析 → 找出差异化和机会点

## 输出格式

分析报告应包含：

1. **核心发现**（1-2句话总结）
2. **数据支撑**（关键指标）
3. **可执行建议**（具体行动步骤）
4. **风险提示**（需要注意的问题）

## 注意事项

1. **数据来源**: 部分接口返回 `dataSource: "lingxi"` 或 `dataSource: "pgy"`，表示数据来自不同平台，正常
2. **降级查询**: 博主详情支持用昵称替代 authorId，会自动降级返回聚合数据
3. **空数据**: 某类目无数据时返回空数组，属正常行为，换其他类目重试
4. **参数说明**: `timeRange` 支持 `"7"` 或 `"30"` 天；趋势分析粒度参数是 `granularity`（非 `interval`）
