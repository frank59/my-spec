# my-spec

AI 协同工作知识资产库，沉淀 Prompts、Skills、Rules 等。

---

## Skills

### diary-generator（日记生成器）

将 Obsidian Inbox 和 Clippings 自动转换为结构化日记的 Skill。

#### 快速安装（For AI Agent）

```markdown
## 任务：安装 diary-generator Skill

### 步骤 1：安装依赖（obsidian-cli）

**依赖信息**:
- 名称: obsidian-cli
- 来源: https://github.com/kepano/obsidian-skills.git
- 用途: 操作 Obsidian Vault

**根据你的 AI Agent 类型安装**:

| AI Agent | 安装命令 |
|---------|---------|
| **Codex CLI** | `npx skills add git@github.com:kepano/obsidian-skills.git` |
| **OpenCode** | `git clone https://github.com/kepano/obsidian-skills.git ~/.opencode/skills/obsidian-skills` |
| **Claude Code** | 将 obsidian-skills 内容添加到 `/.claude` 目录 |
| **其他** | 参考你的 AI Agent 文档安装 skills |

**验证安装**:
```bash
obsidian --version
# 应返回版本号
```

### 步骤 2：安装 diary-generator

**方式 1: 直接克隆（推荐）**
```bash
# 克隆到 AI Agent 的 skills 目录
git clone https://github.com/frank59/my-spec.git /tmp/my-spec
cp -r /tmp/my-spec/skills/diary-generator {your-skills-dir}/
```

**方式 2: 手动下载**
1. 下载 `https://github.com/frank59/my-spec/tree/main/skills/diary-generator`
2. 复制到 skills 目录: `{skills-dir}/diary-generator/`

**验证目录结构**:
```
{skills-dir}/diary-generator/
├── SKILL.md          # 主文档（必需）
├── manifest.json     # 配置和依赖声明
└── test-cases/       # 测试用例
```

### 步骤 3：配置

查看 `diary-generator/manifest.json` 中的 `config` 部分：

| 配置项 | 默认值 | 说明 |
|--------|--------|------|
| `VAULT_PATH` | `/Users/wangheng/work/projects/gitee/obsidian` | Obsidian Vault 路径 |
| `DIARY_WRITE_MODE` | `append` | 写入模式 |
| `CLIPPINGS_ENABLED` | `true` | 启用剪藏收集 |

根据需要修改配置。

### 步骤 4：验证安装

读取 SKILL.md 确认安装成功：
```bash
cat {skills-dir}/diary-generator/SKILL.md
```

### 使用方式

执行 SKILL.md 中描述的步骤：

1. **检查依赖**: `obsidian --version`
2. **生成今日日记**: 按 SKILL.md "执行步骤" 操作
3. **查看输出**: 检查 `05日记/YYYY-MM-DD.md`

### 注意事项

- 必须先安装 obsidian-cli 依赖
- 确保 Obsidian 应用正在运行
- 遵循 Agent Skills 标准格式（SKILL.md + manifest.json）
```

#### 功能特性

| 功能 | 说明 |
|------|------|
| **Inbox 收集** | 支持任意文件名的 Inbox 记录 |
| **Clippings 汇总** | 自动收集当日网页剪藏，生成表格 |
| **增量追加** | 日记已存在则追加，不覆盖手动编辑 |
| **智能归档** | 当月平铺存储，次月自动归档 |
| **双向链接** | 自动关联知识库笔记 |

#### 目录结构

```
skills/diary-generator/
├── README.md              # 完整文档
├── manifest.json          # 依赖声明和配置
└── test-cases/            # 测试用例
    ├── case-1-input.md
    └── case-1-output.md
```

---

## 更多内容

持续更新中...