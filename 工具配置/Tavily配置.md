# Tavily Search API 配置

**最后更新：** 2026-02-03

---

## 📌 配置概述

### Tavily API
- **用途：** AI 优化的搜索引擎，支持网页搜索、研究、查询最新信息
- **API Key：** `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`
- **状态：** ✅ 已配置

---

## 🔧 技能位置

- **技能路径：** `/home/admin/clawd/skills/tavily/SKILL.md`
- **脚本路径：** `/home/admin/clawd/skills/tavily/scripts/tavily_search.py`

---

## ✨ 主要功能

### 搜索模式
- **basic：** 快速搜索（1-2秒），适合简单查询
- **advanced：** 深度搜索（5-10秒），适合研究和复杂主题

### 主题类型
- **general：** 通用搜索（所有时间）
- **news：** 新闻搜索（最近 7 天）

### 高级功能
- AI 生成答案摘要
- 干净的结构化结果
- 域名过滤（包含/排除特定来源）
- 图片搜索
- 原始内容提取

---

## 📝 使用方法

### 命令行使用（需要安装 tavily-python）
```bash
# 基本搜索
python3 scripts/tavily_search.py "查询内容"

# 深度搜索
python3 scripts/tavily_search.py "查询内容" --depth advanced

# 新闻搜索
python3 scripts/tavily_search.py "查询内容" --topic news

# 限制结果数量
python3 scripts/tavily_search.py "查询内容" --max-results 10

# 包含特定域名
python3 scripts/tavily_search.py "查询内容" --include-domains python.org docs.python.com

# 排除特定域名
python3 scripts/tavily_search.py "查询内容" --exclude-domains w3schools.com
```

### 直接 API 调用
```bash
curl -X POST "https://api.tavily.com/search" \
  -H "Content-Type: application/json" \
  -d '{
    "api_key": "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",
    "query": "你的查询内容",
    "search_depth": "basic",
    "max_results": 10,
    "include_answer": true
  }'
```

---

## ⚠️ 注意事项

1. **Python 包未安装：**
   - 需要安装 `tavily-python` 包
   - 当前环境 pip 安装时遇到依赖问题（tiktoken 版本冲突）
   - 建议使用直接 API 调用方式

2. **成本控制：**
   - 使用 `basic` 深度节省额度
   - 限制 `max_results` 到实际需要的数量
   - 实现本地缓存减少重复查询

---

## 🔗 相关链接

- Tavily 官网：https://tavily.com
- API 文档：https://docs.tavily.com
- 技能文档：`/home/admin/clawd/skills/tavily/SKILL.md`

---

*配置完成日期：2026-02-03*
