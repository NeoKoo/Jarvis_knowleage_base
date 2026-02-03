# GitHub 知识库配置

**最后更新：** 2026-02-03

---

## 📌 配置概述

### GitHub 仓库
- **仓库地址：** https://github.com/NeoKoo/Jarvis_knowleage_base
- **本地路径：** `/home/admin/clawd/knowledge-base/`
- **远程 URL：** `git@github.com:NeoKoo/Jarvis_knowleage_base.git` (SSH)
- **默认分支：** `main`

### 配置方式
- **同步方式：** 只同步 `knowledge-base/` 文件夹（不包含整个工作区）
- **认证方式：** SSH 密钥
- **Git 配置：**
  - 用户名：`Jarvis AI`
  - 邮箱：`jarvis@ai-assistant.local`

---

## 📁 文件结构

```
knowledge-base/
├── .git/                          # Git 仓库配置
├── .gitignore                      # Git 忽略规则
├── README.md                       # 知识库说明文档
├── MEMORY.md                       # 长期记忆
├── AGENTS.md                       # 项目文档
├── 编程开发/                       # 编程开发相关
│   └── Figma转React方案.md
├── 工具配置/                       # 工具配置记录
│   ├── 钉钉配置.md
│   ├── Tavily配置.md
│   └── GitHub知识库配置.md
├── 项目决策/                       # 项目决策记录
│   └── 知识库同步方案.md
└── 今日总结/                       # 每日对话总结
    └── 2026-02-03.md
```

---

## 🔄 同步工作流

### 自动同步（推荐）
当有重要内容需要记录时，我会自动：
1. 写入相应的文件
2. 执行 `git add` 添加更改
3. 执行 `git commit` 提交
4. 执行 `git push` 推送到 GitHub

### 手动同步
如果需要手动同步：

```bash
cd /home/admin/clawd/knowledge-base

# 查看状态
git status

# 添加所有更改
git add .

# 提交（写清楚描述）
git commit -m "Update: 描述你的更改"

# 推送到 GitHub
git push
```

### 查看历史
```bash
# 查看提交历史
git log --oneline

# 查看最近 5 次提交的详细信息
git log -5 --stat

# 查看某个文件的历史
git log --follow 文件名
```

---

## 🔧 Git 配置

### 全局配置
```bash
git config --global user.name "Jarvis AI"
git config --global user.email "jarvis@ai-assistant.local"
```

### 本地配置
```bash
# 查看远程仓库
git remote -v

# 修改远程仓库 URL
git remote set-url origin <new-url>

# 查看分支
git branch -a
```

---

## ⚠️ 注意事项

1. **SSH 密钥已配置**
   - 位置：`~/.ssh/id_ed25519`
   - 测试：`ssh -T git@github.com` ✅ 成功

2. **文件权限**
   - 确保知识库文件可写
   - `.git` 文件夹不要手动修改

3. **同步冲突**
   - 如果遇到冲突，使用 `git pull --rebase` 解决
   - 或者使用 `git pull --no-rebase` 创建合并提交

4. **敏感信息**
   - 不要在知识库中保存密码、token 等敏感信息
   - 使用 `.gitignore` 排除敏感文件

---

## 🎯 使用建议

### 分类原则
- **编程开发/**：技术方案、工具对比、代码示例
- **工具配置/**：各工具的配置信息、API key（非敏感部分）
- **项目决策/**：重要决策、方案选择、架构设计
- **今日总结/**：每日对话总结、重要事项记录

### 命名规范
- 文件名使用中文描述（因为用户是中文用户）
- 使用 `.md` 扩展名
- 重要决策文件在文件名中包含日期
- 长期文档放在对应分类下，不加日期

### 提交信息规范
```
# 格式
<类型>: <描述>

# 类型示例
Update: 更新现有内容
Add: 新增文件/内容
Fix: 修复错误
Refactor: 重构内容
Docs: 文档更新

# 示例
Update: 更新 Figma 转 React 方案对比
Add: 新增钉钉配置文档
Fix: 修复 GitHub 知识库同步问题
```

---

## 🔗 相关链接

- GitHub 仓库：https://github.com/NeoKoo/Jarvis_knowleage_base
- Git 文档：https://git-scm.com/doc
- SSH 密钥配置：https://docs.github.com/zh/authentication/connecting-to-github-with-ssh

---

*配置完成日期：2026-02-03*
*首次推送：2026-02-03 00:03*
