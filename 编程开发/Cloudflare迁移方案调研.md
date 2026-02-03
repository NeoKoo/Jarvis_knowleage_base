# Cloudflare/MoltWorker 迁移方案

**记录日期：** 2026-02-03
**来源：** 用户提供的 GitHub 仓库：cloudflare/moltworker

---

## 📋 项目概览

### 项目定位
**OpenClaw on Cloudflare Workers** - 将 OpenClaw（原名 Moltbot/Clawdbot）迁移到 Cloudflare Workers 平台运行。

### 为什么选择 Cloudflare Workers？
- ✅ **无服务器维护成本** - 无需管理自己的服务器
- ✅ **全球分布** - Cloudflare 边缘网络覆盖全球，延迟更低
- ✅ **冷启动速度** - Cloudflare Workers 冷启动 ~10-15 秒，后续请求更快
- ✅ **高可靠性** - Cloudflare 基础设施，99.99% SLA
- ✅ **AI Gateway 集成** - 可直接使用 Anthropic API，通过 AI Gateway 路由
- ✅ **免费额度充足** - Cloudflare Workers Free Plan 每天 10 万次免费请求
- ✅ **浏览器自动化 (CDP）** - 内置 Chrome DevTools Protocol 支持

---

## 🏗️ 架构对比

### 之前架构（设备配对）
```
用户设备 (iOS/macOS/Android/Windows)
    ↓
OpenClaw Gateway (macOS 服务器/本地)
    ↓
设备配对 (需要手动在 OpenClaw 中批准)
    ↓
云服务 (钉钉/Telegram/等)
```

**问题：**
- ❌ 需要本地服务器（Mac/PC）一直运行
- ❌ 设备配对需要手动批准
- ❌ 维护成本高（电费、服务器费用）
- ❌ 单点故障风险
- ❌ 无法异地访问

### 新架构（Cloudflare Workers）
```
用户设备 (浏览器/移动设备)
    ↓
Cloudflare Workers (全球分布)
    ↓
Anthropic API (AI Gateway)
    ↓
OpenClaw (运行在 Workers 中)
```

**优势：**
- ✅ **无服务器维护** - Cloudflare 基础设施，零维护
- ✅ **全球分布** - 边缘网络，低延迟
- ✅ **高可靠性** - 99.99% SLA
- ✅ **AI Gateway 原生** - 更好的模型支持
- ✅ **浏览器自动化** - 通过 CDP 操控浏览器
- ✅ **免费额度** - 每天 10 万次免费请求
- ✅ **控制台** - 通过 Cloudflare Dashboard 管理

---

## 🔧 核心功能

### 1. 浏览器自动化（CDP）
**Chrome DevTools Protocol (CDP）** 内置支持

**主要能力：**
- 📸 截图
- 🎬 视频（多 URL 合成）
- 📜 点击和输入
- 🌐 页面导航
- 📝 表单填充
- 📊 DOM 操作（查询、修改）

**使用场景：**
- 网页抓取
- 自动化测试
- 表单自动填写
- 定期监控和截图

### 2. AI Gateway 集成
**Anthropic API via AI Gateway**

**配置方式：**
```bash
# 设置 AI Gateway（可选，但推荐）
npx wrangler secret put AI_GATEWAY_API_KEY sk-ant-xxx

# 或者使用 Cloudflare AI Gateway（更简单）
npx wrangler secret put CF_ACCESS_TEAM_DOMAIN myteam.cloudflareaccess.com
npx wrangler secret put CF_ACCESS_AUD aud
```

**支持的 AI Provider：**
- Anthropic (Claude)
- OpenAI
- Google Gemini
- Minimax
- Together AI

**使用示例：**
```bash
# 生成代码
curl -X POST "$AI_GATEWAY_ENDPOINT" \
  -H "Authorization: Bearer $AI_GATEWAY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"model":"claude-sonnet-4","max_tokens":4096}'
```

### 3. R2 持久存储（Cloudflare D1）
**默认存储：** 在 Cloudflare Workers 沙盒中，数据在容器重启后会丢失

**R2 配置：**
```bash
# 创建 R2 存储空间
npx wrangler d1 create "moltbot-data"

# 挂载到 Workers
npx wrangler d1 mount "moltbot-data" \
  --database-id="YOUR_R2_DATABASE_ID" \
  --database-name="moltbot-data"

# 设置环境变量
npx wrangler secret put MOLTBOT_D1_DATABASE_ID your_db_id
npx wrangler secret put MOLTBOT_D1_DATABASE_NAME your_db_name
```

**优势：**
- ✅ 数据持久化（容器重启不丢失）
- ✅ 全球分布读取
- ✅ 免费额度高（R2 有 100 GB 免费额度/天）

**限制：**
- ⚠️ 需要付费计划（超过免费额度后 $0.15/GB-月）
- ⚠️ R2 有 50ms 读取延迟

### 4. 控制台和 API

**Cloudflare Dashboard 控制台：**
- 📊 实时日志查看
- 📈 性能监控
- 🔐 部署管理
- 🌐 环境变量配置
- 🔐 API Keys 管理
- 💾 R2 存储管理
- 🤖 Workers 管理

**Admin API（/_admin/）：**
- R2 状态查询
- 备份和恢复
- 立即重启
- 批量操作

**Worker API：**
- 创建新的 Worker
- 更新 Worker 代码
- 查看 Worker 日志
- 绑定自定义域名

**调试端点（/debug/）：**
- 进程列表
- 日志查看
- 版本信息
- 性能指标

---

## 🚀 部署方式

### 方式 A：完全迁移到 Cloudflare Workers（推荐）
```bash
# 1. Fork 仓库
git clone https://github.com/cloudflare/moltworker.git

# 2. 安装依赖
cd cloudflare-moltworker
npm install

# 3. 配置 Cloudflare Access
npx wrangler secret put CF_ACCESS_TEAM_DOMAIN
npx wrangler secret put CF_ACCESS_AUD

# 4. 配置 R2 存储（可选）
npx wrangler d1 create "moltbot-data"

# 5. 配置 Worker
npx wrangler deploy

# 6. 设置环境变量
npx wrangler secret put AI_GATEWAY_API_KEY
npx wrangler secret put AI_GATEWAY_BASE_URL
```

**优点：**
- ✅ 零服务器维护
- ✅ 全球分布
- ✅ 高可靠性
- ✅ 零成本

**缺点：**
- ❌ 部署复杂度增加
- ❌ 需要学习 Cloudflare Workers 生态
- ❌ 调试可能更困难

### 方式 B：本地开发模式（用于快速迭代）
```bash
# 1. Fork 仓库到自己的 GitHub
git clone https://github.com/cloudflare/moltworker.git your-username/moltworker

# 2. 安装依赖
cd moltworker
npm install

# 3. 本地开发运行
npm run dev

# 4. 本地调试
npm run dev:inspect

# 5. 部署到 Cloudflare Workers
npx wrangler deploy --env production
```

**优点：**
- ✅ 快速迭代
- ✅ 本地调试方便
- ✅ 可使用熟悉的工具

**缺点：**
- ❌ 仍然需要本地服务器
- ❌ 依赖 Cloudflare 基础设施

---

## 📊 成本和额度分析

### Cloudflare Workers Free Plan
- **免费请求：** 每天 10 万次
- **免费读取：** R2 存储每天 100 GB
- **免费写入：** R2 存储每天 100 GB

### 估算成本（按月）
**轻量使用（每天 1 万次请求）：**
- Cloudflare Workers 免费
- R2 存储（基本不使用）：免费
- 总成本：$0

**中度使用（每天 5 万次请求）：**
- Cloudflare Workers 免费
- R2 存储（100 GB/天）：$0.15/GB/天 * 30 = $4.5/月
- 总成本：$4.5/月

**重度使用（每天 10 万次请求）：**
- Cloudflare Workers 免费
- R2 存储（100 GB/天）：$0.15/GB/天 * 30 = $4.5/月
- 总成本：$4.5/月

**超出免费额度：**
- 超过 10 万次请求：$0.25/百万次请求 = $2.5/月
- R2 存储：$0.15/GB/天 * 30 = $4.5/月
- 总成本：$4.5/月 + 存储费用

---

## 💡 迁移建议

### 适合迁移的场景
- ✅ 追求高可用性和可靠性
- ✅ 需要全球分布
- ✅ 预算充足（愿意付费）
- ✅ 需要 AI Gateway 集成

### 不适合迁移的场景
- ✅ 快速原型迭代，本地开发为主
- ✅ 不需要 AI Gateway 额外成本
- ✅ 主要用于内网工具或个人助手

---

## 📝 快速参考

### 常用命令
```bash
# 部署到 Cloudflare Workers
npx wrangler deploy

# 查看部署状态
npx wrangler deployments list

# 查看实时日志
npx wrangler tail

# 进入调试模式
npm run dev

# 配置环境变量
npx wrangler secret put
```

### API 示例
```bash
# 通过 Admin API 配置 R2
curl -X POST "https://api.cloudflare.com/client/v4/accounts/{account_id}/workers/d1" \
  -H "Authorization: Bearer $API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"database_name":"moltbot-data"}'
```

---

## 🔗 相关链接

- **GitHub 仓库：** https://github.com/cloudflare/moltworker
- **官方文档：** https://developers.cloudflare.com/sandbox/
- **AI Gateway 文档：** https://developers.cloudflare.com/ai-gateway/
- **Cloudflare Dashboard：** https://dash.cloudflare.com/
- **Admin API 文档：** https://developers.cloudflare.com/api/

---

## ⚠️ 重要注意事项

### 配置管理
- **API Keys**：安全存储在 Cloudflare Workers Secrets 中
- **环境变量**：使用 `npx wrangler secret put` 配置
- **不要硬编码**：永远不要在代码中写死 key

### 数据持久化
- **R2 存储**：如果需要数据持久化，必须使用 R2
- **默认存储**：Workers KV 无持久化，容器重启丢失

### AI Gateway 配置
- **Provider 选择**：默认使用 Anthropic（Claude）
- **Token 管理**：通过 Admin API 配置，不要暴露
- **速率限制**：注意 AI Gateway 的速率限制

### 调试技巧
- **实时日志**：使用 `npx wrangler tail` 查看最新日志
- **本地开发**：使用 `npm run dev` 本地调试
- **版本控制**：在 package.json 中管理版本号

---

## 🎯 迁移检查清单

在开始迁移前，请确认：

- [ ] 是否需要保留现有的设备配对功能？
- [ ] 是否有重要的自定义功能需要迁移？
- [ ] 预算范围是多少？（是否愿意付费）
- [ ] 是否需要 R2 持久存储？
- [ ] 是否已准备好迁移到 Cloudflare Workers？
- [ ] 是否有足够的 Cloudflare 使用经验？

---

*最后更新：2026-02-03*
