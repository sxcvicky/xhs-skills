# 市场洞察数据接口

用于行业趋势分析、品类机会发现、市场格局洞察。

## 基础信息

- **Base URL**: `https://xhsnative-backend-171452-7-1367409358.sh.run.tcloudbase.com`
- **认证**: `X-API-Key: YOUR_API_KEY`

---

## 健康检查

### 检查服务状态

```
GET /api/health
```

**响应**:
```json
{
  "code": 200,
  "data": {
    "status": "ok",
    "uptime": 86400,
    "database": "connected"
  }
}
```

**使用场景**:
- 验证 API 服务可用性
- 调试连接问题
