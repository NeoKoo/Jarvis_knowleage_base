# Figma 转 React 代码方案调研

**日期：** 2026-02-03

---

## 📌 需求背景

用户提供了 Figma 设计稿，需要免费且好用的方案将设计转换为 React 代码。

---

## 🎯 方案对比

### 方案一：传统 Figma-to-Code 工具

#### 1. Locofy.ai ⭐⭐⭐⭐⭐
- **价格：** 免费版可用，付费版 $29-79/月
- **特点：**
  - AI 驱动，生成干净、模块化的 React 代码
  - 支持 React/Next.js、Tailwind
  - 响应式设计 + 动画支持
  - 对开发者友好
- **适用场景：** 需要高质量生产级代码

#### 2. Builder.io ⭐⭐⭐⭐⭐
- **价格：** 免费版可用，Pro 版 $49+/月
- **特点：**
  - Visual CMS + Figma 转 React
  - 学习你的代码模式和设计系统
  - 生成生产就绪的组件
  - 可编辑性极强
- **适用场景：** 需要 CMS 集成或团队协作

#### 3. PlatUI ⭐⭐⭐⭐ (完全免费)
- **价格：** 完全免费 ✅
- **特点：**
  - Figma 社区插件
  - 支持 React、Flutter、HTML/CSS、React Native
  - AI 驱动，生成响应式代码
- **适用场景：** 预算有限，需要免费方案

#### 4. Visual Copilot ⭐⭐⭐⭐
- **价格：** 部分功能免费
- **特点：**
  - Figma 社区插件，安装方便
  - 支持 React/Tailwind 等多种格式
  - 快速转换
- **适用场景：** 快速原型验证

#### 5. Anima ⭐⭐⭐
- **价格：** 免费试用，$31+/月
- **特点：**
  - 支持响应式 React
  - 支持 Styled-Components
  - 但生成的代码通常需要清理
- **适用场景：** 快速 MVP 原型

#### 6. Figma Inspect + 手动编码 ⭐⭐⭐
- **价格：** Figma 免费版自带 ✅
- **特点：** 100% 免费，完全控制代码质量
- **适用场景：** 追求代码质量，不介意手动编码

---

### 方案二：Pixso MCP（AI 编程助手）

**核心概念：** Pixso 设计工具 + MCP 协议 + AI 编程助手（Cursor/Windsurf/Claude）

#### 工作原理
```
Pixso 设计文件 ← MCP 协议 → AI 编程助手 → 生成符合项目规范的 React 代码
```

#### 优点
- ✅ AI 能实时读取设计上下文
- ✅ 生成的代码符合项目规范
- ✅ 可以在 IDE 中持续交互和调整
- ✅ 支持复杂的设计系统

#### 缺点
- ❌ 需要安装 Pixso 客户端
- ❌ 需要配置 AI 编程工具
- ❌ 学习曲线稍陡

#### 使用流程

**Step 1：安装并配置 Pixso**
1. 下载 Pixso 客户端：https://pixso.net/download/
2. 创建或打开设计文件
3. 在「文件」菜单中启用「Pixso MCP」
4. 本地服务器地址：`http://localhost:3667/mcp`

**Step 2：配置 AI 编程工具**

**选项 A：Cursor（推荐）**
```
Cursor Settings → MCP & Integrations → New MCP Server
配置：
{
  "mcpServers": {
    "Pixso MCP": {
      "url": "http://localhost:3667/mcp",
      "headers": {}
    }
  }
}
```

**选项 B：VS Code**（需 v1.99+）
```
Open Chat (Ctrl+Alt+I) → Configure Tools → Add MCP Server → HTTP
配置：
{
  "servers": {
    "pixso": {
      "url": "http://localhost:3667/mcp",
      "type": "http"
    }
  }
}
```

**选项 C：Windsurf**
```
Settings → MCP Servers → Manage MCPs → View raw config
配置：
{
  "mcpServers": {
    "pixso": {
      "serverUrl": "http://localhost:3667/mcp"
    }
  }
}
```

**选项 D：Claude（命令行）**
```bash
claude mcp add --transport http pixso-desktop http://127.0.0.1:3667/mcp
```

**Step 3：开始使用**

**方法一：复制链接**
1. 在 Pixso 中选择 Frame 并复制链接
2. 在 AI 编程工具中粘贴链接
3. 给出指令：「生成 React 代码」

**方法二：选择 Frame**
1. 在 Pixso 中选择单个 Frame 图层
2. 在 AI 编程工具中直接交互
3. 例如：「生成 React 代码」

#### 使用建议
1. 保持 Pixso 客户端运行，设计文件在活动标签页
2. 使用高级模型（Claude 4.0 Sonnet、Qwen3 等）
3. 一次只选一个 Frame
4. 复杂设计建议分块处理

---

## 📊 核心区别对比

| 维度 | 传统 Figma-to-Code | Pixso MCP |
|------|-------------------|-----------|
| **核心定位** | 设计 → 代码转换 | 设计 → AI 助手 → 代码 |
| **代码质量** | 通用模板，需调整 | 符合项目规范 |
| **自由度** | 低（依赖工具） | 高（AI 理解上下文） |
| **上手难度** | 简单 | 中等 |
| **适用场景** | 快速原型、静态页面 | 生产环境、复杂项目 |
| **是否换设计工具** | 否（继续用 Figma） | 是（需用 Pixso） |
| **与 IDE 集成** | 弱 | 强（实时在 IDE 中） |

---

## 🎯 推荐选择

### 选择传统工具，如果：
- ✅ 用户已经在用 Figma，不想换工具
- ✅ 项目简单，只需要快速生成静态 UI
- ✅ 不想配置复杂的开发环境
- ✅ 追求开箱即用

### 选择 Pixso MCP，如果：
- ✅ 可以接受切换到 Pixso
- ✅ 需要生成高质量、符合项目规范的代码
- ✅ 已经在使用 AI 编程工具（Cursor、Windsurf 等）
- ✅ 项目复杂，有设计系统和组件库
- ✅ 追求长期开发效率和代码一致性

---

## 🔗 相关链接

- Locofy.ai：https://www.locofy.ai/convert/figma-to-react
- Builder.io：https://www.builder.io/blog/convert-figma-to-react-code
- Pixso MCP：https://pixso.net/articles/pixso-mcp/
- Figma 官方 MCP 指南：https://help.figma.com/hc/en-us/articles/32132100833559

---

*最后更新：2026-02-03*
