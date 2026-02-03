# GitHub 库分析工作流

**创建日期：** 2026-02-03
**目的：** 规范化处理用户提供的 GitHub 地址/开源库的分析流程

---

## 📋 工作流程

当用户提供 GitHub 仓库地址或开源库名称时：

### 步骤 1：识别来源类型

通过 URL 或名称判断是什么类型：

| 来源类型 | 说明 | 例子 |
|---------|------|------|
| **官方文档** | GitHub 官方仓库的文档 | docs.github.com |
| **开源项目** | GitHub 上的开源代码仓库 | github.com/username/repo |
| **Awesome 列表** | 资源合集 | awesome-* 仓库 |
| **Skills/Claudius** | AI 助手技能 | github.com/openclaw/skills/ |
| **讨论/Issue** | 项目的讨论 | github.com/username/repo/issues |
| **Wiki/知识库** | 项目文档 | github.com/username/repo/wiki |

### 步骤 2：获取基本信息

对 GitHub 仓库执行以下查询：

```bash
# 查看仓库基本信息（描述、语言、许可）
gh repo view <owner/repo>

# 查看 README
gh api /repos/<owner>/<repo>/readme

# 查看仓库统计（Star、Fork、贡献者）
gh api /repos/<owner>/<repo>/traffic

# 列出最近的提交
gh api /repos/<owner>/<repo>/commits
```

### 步骤 3：分析仓库内容

#### 3.1 技术栈分析
- **主要语言**：查看仓库使用的主要编程语言
- **依赖管理**：package.json, requirements.txt, go.mod 等
- **框架/库**：React, Vue, Django, Flask 等

#### 3.2 项目结构分析
- **文档组织**：docs/, wiki/, README.md
- **代码结构**：src/, lib/, pkg/
- **配置文件**：.github/, config/, docker-compose.yml

#### 3.3 活跃度分析
- **最近提交**：判断项目是否活跃维护
- **Issue 处理**：查看未关闭的 Issue 数量
- **Release 版本**：查看最近发布的版本

### 步骤 4：生成笔记记录

创建结构化的 Markdown 笔记，包含：

```markdown
# [仓库名称] 分析

## 基本信息
- **地址：** https://github.com/owner/repo
- **所有者：** @username
- **描述：** [从仓库获取]
- **主要语言：** [从仓库获取]
- **Stars：** [从仓库获取]

## 技术栈
- **语言：** [列表]
- **框架：** [列表]
- **主要依赖：** [列表]

## 项目结构
- **目录组织：** [描述]
- **关键文件：** [列出重要文件]

## 特点分析
- [优点]：[列出]
- [注意事项]：[列出]

## 适用场景
- 这个库适合做什么？
- 为什么选择它而不是其他方案？
- 需要注意的配置或依赖

## 关键发现
- [列出重要的技术决策、API 用法、最佳实践等]

## 链接
- README: [README URL]
- Issues: [Issues URL]
- Wiki: [Wiki URL]
```

### 步骤 5：保存到知识库

使用 `obsidian-cli create` 将笔记保存到对应文件夹：

```bash
# 如果是技术/编程相关的仓库
obsidian-cli create "编程开发/[仓库名称].md" --content "# 标题\n\n内容"

# 如果是通用的资源或列表
obsidian-cli create "编程开发/GitHub/[仓库名称].md" --content "# 标题\n\n内容"
```

---

## 🔍 快速参考命令

### 获取仓库信息
```bash
# 查看仓库详情
gh repo view owner/repo

# 查看仓库语言
gh api repos/owner/repo/languages

# 查看最近活动
gh api repos/owner/repo/events
```

### 查看 README
```bash
# 使用 gh 命令查看
gh api repos/owner/repo/readme

# 或使用 web_fetch
web_fetch https://raw.githubusercontent.com/owner/repo/main/README.md
```

### 搜索 Issue/讨论
```bash
# 列出所有 Issues
gh issue list owner/repo

# 搜索特定关键词
gh issue list owner/repo --search "关键词"

# 查看特定 Issue
gh issue view owner/repo/issue_number
```

### 查看贡献者
```bash
gh api repos/owner/repo/contributors
```

---

## 📝 示例笔记模板

### 技术库分析
```markdown
# [库名称] 技术分析

## 基本信息
- **地址：** https://github.com/owner/repo
- **描述：** 一个高性能的...

## 技术栈
- **主要语言：** TypeScript, Go
- **框架：** Express.js, React
- **数据库：** PostgreSQL, Redis

## 项目特点
- ✅ 高性能
- ✅ 易于集成
- ⚠️ 需要 Node.js 18+

## 关键发现
- 使用 Redis 缓存提高性能
- WebSocket 支持实时更新
- 提供 Docker 部署方案
```

### Awesome 列表分析
```markdown
# [Awesome 列表名称] 资源集合

## 概述
- **地址：** https://github.com/username/awesome-list
- **分类数：** 20+ 分类

## 值得关注的项目
- [列出最重要的 3-5 个项目]

## 快速访问
- 按首字母浏览 README
- 使用 Ctrl+F 搜索关键词
```

---

## ⚠️ 注意事项

1. **隐私信息**：不要记录 API Key、密码等敏感信息
2. **准确性**：对 API 返回的数据进行合理推测，不盲目记录
3. **时效性**：记录分析日期，定期回顾更新
4. **分类清晰**：使用有意义的文件名和文件夹结构

---

*最后更新：2026-02-03*
