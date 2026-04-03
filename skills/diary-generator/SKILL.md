---
name: diary-generator
description: 将 Obsidian Inbox 和 Clippings 自动转换为结构化日记
version: 1.5.1
author: frank59
source: https://github.com/frank59/my-spec/tree/main/skills/diary-generator
dependencies:
  - name: obsidian-cli
    source: https://github.com/kepano/obsidian-skills.git
    required: true
tools:
  - obsidian-cli
categories:
  - productivity
  - knowledge-management
  - obsidian
---

# 日记生成器 (Diary Generator)

## 描述

自动将 Obsidian Inbox 中的原始记录和 Clippings 网页剪藏转换为结构化的每日日记，支持增量追加和智能归档。

## 使用场景

- 每日结束时，将零散的 Inbox 记录整理成结构化日记
- 自动汇总当天浏览的网页剪藏
- 建立日记与知识库的双向链接
- 支持多次生成，不覆盖手动编辑内容

## 输入

### 来源 1: Inbox 原始记录

**位置**: `{VAULT_PATH}/00Inbox/`

**支持格式**:
- 任意文件名（通过 frontmatter 或内容识别日期）
- 每行以 `- ` 开头的记录

**示例**:
```markdown
---
created: 2026-04-02 11:01
---

- 9:00 重构了订单模块的并发处理逻辑
- 想法：是不是可以把这部分抽象成通用组件？
- 收获：读写锁在读多写少场景下性能提升很明显
```

### 来源 2: Clippings 网页剪藏

**位置**: `{VAULT_PATH}/00Inbox/`（与 Inbox 共用目录）

**识别方式**:
- 通过 frontmatter 中的 `source: web` 或 `type: clipping` 标记识别剪藏内容
- 或通过文件名前缀识别（如 `clip-`、`剪藏-`）
- 通过 `created` 或 `date` frontmatter 识别日期

## 输出

### 日记文件位置

**当月日记**（活跃）:
```
{VAULT_PATH}/05日记/YYYY-MM-DD.md
```

**归档日记**（次月自动归档）:
```
{VAULT_PATH}/05日记/YYYY/MM/YYYY-MM-DD.md
```

### 输出格式

```markdown
---
created: YYYY-MM-DD HH:mm
tags: 日记
date: YYYY-MM-DD
---

## 今日概览
一句话总结今天的主要活动和收获。

## 做的事
- [时间] 具体事项

## 想法
- 灵感/问题描述

## 收获
- 学到的知识点

## 学习输入
- 阅读内容

## 网页剪藏

| 标题 | 摘要 | 链接 |
|------|------|------|
| 文章标题 | 摘要内容 | [[剪藏文件名]] |

## 明日关注
- [ ] 待办事项

## 相关链接
- [[相关笔记]]
```

## 执行步骤

### Step 1: 检查依赖

验证 obsidian-cli 已安装：
```bash
obsidian --version
```

### Step 2: 收集 Inbox 记录

扫描 `00Inbox/` 目录，识别当日记录文件：
```bash
obsidian search path="00Inbox" query="created:today" --json
```

解析记录内容，按类型分类：
- **做的事**: 无前缀的记录
- **想法**: 以 `想法：` 开头的记录
- **收获**: 以 `收获：` 开头的记录
- **学习输入**: 以 `看了/读了` 开头的记录

### Step 3: 收集 Clippings

在 `00Inbox/` 中识别剪藏文件：
```bash
obsidian search path="00Inbox" query="created:today" --json
```

**筛选剪藏文件**（满足以下任一条件）：
- frontmatter 包含 `source: web` 或 `type: clipping`
- 文件名以 `clip-`、`剪藏-` 等前缀开头
- 内容包含网页 URL（`http://` 或 `https://`）

**提取每个剪藏的**：
- 标题（来自 frontmatter 的 `title` 或文件名）
- 摘要（正文前 200 字或 `description` frontmatter）
- 文件名（用于生成双向链接）

### Step 4: 检查日记是否存在

```bash
obsidian read path="05日记/YYYY-MM-DD.md"
```

### Step 5: 生成或追加日记

**如果日记不存在** - 创建新日记：
```bash
obsidian create path="05日记/YYYY-MM-DD.md" content="..."
```

**如果日记已存在** - 追加到对应章节：
```bash
obsidian append path="05日记/YYYY-MM-DD.md" content="..."
```

### Step 6: 建立知识链接

识别技术关键词，生成 Wikilink：
- 扫描 `02领域/01计算机/` 目录下的现有笔记
- 匹配技术术语（Redis、DDD、微服务等）
- 在日记中生成 `[[笔记名]]` 链接

## 配置

默认配置（可在 manifest.json 中修改）：

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `VAULT_PATH` | `/Users/wangheng/work/projects/gitee/obsidian` | Obsidian Vault 路径 |
| `DIARY_WRITE_MODE` | `append` | 写入模式：append/create/overwrite |
| `CLIPPINGS_ENABLED` | `true` | 是否启用 Clippings 收集 |
| `ARCHIVE_ENABLED` | `true` | 是否启用自动归档 |

## 使用示例

### 生成今日日记

```bash
# 自动判断追加或创建
qoder skill diary-generator --date today
```

### 生成指定日期日记

```bash
qoder skill diary-generator --date 2026-04-02
```

### 处理特定 Inbox 文件

```bash
qoder skill diary-generator --inbox-file "突发想法.md"
```

### 手动归档

```bash
# 归档指定月份
qoder skill diary-generator --archive 2026-03

# 归档上个月
qoder skill diary-generator --archive-last-month
```

## 注意事项

1. **依赖要求**: 必须先安装 obsidian-cli
2. **Obsidian 运行**: 确保 Obsidian 应用正在运行
3. **增量追加**: 默认追加模式不会覆盖手动编辑内容
4. **链接有效性**: 自动生成的 Wikilink 可能指向不存在的笔记

## 版本历史

- v1.0 (2026-04-02): 初始版本
- v1.1 (2026-04-02): 添加测试用例
- v1.2 (2026-04-02): 当月平铺、次月归档策略
- v1.3 (2026-04-02): Inbox 任意命名支持
- v1.4 (2026-04-02): 增量追加模式
- v1.5 (2026-04-02): 网页剪藏自动收集
- v1.5.1 (2026-04-02): 遵循 Agent Skills 标准重构
