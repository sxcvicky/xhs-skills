# 小红书数据 API Skill

通过 HTTP API 查询小红书平台的话题、博主、笔记、关键词等数据。

## 安装方式

### 方式 1：手动安装

```bash
# 复制 Skill 到 OpenClaw 全局技能目录
cp -r xhs-data-api ~/.openclaw/skills/

# 或复制到项目目录
cp -r xhs-data-api <your-project>/.claude/skills/
```

### 方式 2：通过 ClawHub 安装

```bash
# 安装 clawhub CLI
npm install -g clawhub

# 登录
clawhub login

# 安装 Skill
clawhub install xhs-data-api
```

## 配置

在 `openclaw.json` 中配置 API Key：

```json
{
  "skills": {
    "entries": {
      "xhs-data-api": {
        "enabled": true,
        "env": {
          "XHS_API_KEY": "your-api-key-here"
        }
      }
    }
  }
}
```

## 验证安装

```bash
# 查看已加载的 Skill
openclaw skills list --eligible

# 详细视图
openclaw skills list --verbose
```

## 使用示例

### 查询种子池统计

```bash
curl -X GET "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/crawler/mkt/seed-stats" \
  -H "X-API-Key: $XHS_API_KEY"
```

### 查询话题列表

```bash
curl -X GET "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/query/contentcapital/topics?page=1&pageSize=20" \
  -H "X-API-Key: $XHS_API_KEY"
```

## 目录结构

```
xhs-data-api/
├── SKILL.md                    # Skill 主文件
├── references/
│   └── api-endpoints.md        # 完整接口文档
└── README.md                   # 本文件
```

## 触发关键词

当用户提到以下关键词时，此 Skill 会被激活：
- 小红书话题 / XHS topics
- 博主分析 / blogger insights
- 笔记数据 / note performance
- 关键词查询 / keyword search
- 内容策略 / content strategy
- 竞品分析 / competitor analysis
- 种子池 / seed pool
- 爬虫状态 / crawler status

## 许可证

MIT
