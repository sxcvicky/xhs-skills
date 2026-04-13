# 小红书数据分析 Skill

通过数据驱动的方法，帮助用户制定小红书内容策略、分析竞品、洞察受众。

## 这个 Skill 能做什么

✅ **内容策略分析**: 找到热门话题中的差异化机会  
✅ **博主合作评估**: 基于互动率和受众匹配度推荐博主  
✅ **竞品分析**: 对比竞品内容策略，找出机会点  
✅ **数据解读**: 将原始数据转化为可执行建议  

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

### 内容策略分析

```bash
# 查询热门话题
curl -X GET "https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com/api/query/contentcapital/topics?page=1&pageSize=20" \
  -H "X-API-Key: $XHS_API_KEY"
```

## 目录结构

```
xhs-data-api/
├── SKILL.md                    # Skill 主文件（思维框架）
├── references/
│   ├── data-content.md         # 内容策略数据接口
│   ├── data-bloggers.md        # 博主数据接口
│   ├── data-competitive.md     # 竞品分析数据接口
│   └── data-market.md          # 市场洞察数据接口
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
- 受众画像 / audience profile
- 市场趋势 / market trends

## 许可证

MIT
