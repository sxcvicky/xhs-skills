# Detail 详情层接口

> ✅ **已验证** (2026-04-13)

## 接口列表

### 1. 笔记详情

```
GET /api/v1/content/notes/:noteId
```

**状态**: ✅ 可用

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
- `noteId`: 笔记ID
- `title`: 标题
- `author`: 作者昵称
- `type`: 笔记类型（image/video/unknown）
- `exposure`: 曝光量
- `reads`: 阅读量
- `interactions`: 互动量
- `ctr`: 点击率
- `engageRate`: 互动率
- `fullViewsRate`: 完播率（视频笔记）
- `posterUrl`: 封面图URL
- `linkUrl`: 笔记链接
- `brand`: 合作品牌
