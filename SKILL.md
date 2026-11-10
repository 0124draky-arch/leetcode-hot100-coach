---
name: leetcode-hot100-coach
description: 初始化一个 LeetCode Hot 100 手写训练项目。当用户想系统刷 Hot 100、准备算法面试、需要手写代码训练，或明确输入 /leetcode-hot100-coach 时触发。初始化会生成 COACH.md、progress.md、learning-notes.md 和题单快照；之后由这些文件驱动，不需要重复加载本 skill。不适用于单点题目提问（如“这道题怎么做”）或普通开发需求。
---

# LeetCode Hot 100 手写教练（初始化器）

本 skill **只负责初始化**。初始化完成后，后续教学、存档、读档、复习全部由用户目录里的 `COACH.md` 驱动，不需要再次加载本 skill。

---

## 1. 触发后的第一件事

检查当前工作目录是否存在：

- `COACH.md`
- `progress.md`
- `learning-notes.md`

### 全部存在

说明已经初始化过。**短路退出**，不要重复初始化。告诉用户：

> 这个目录已经有完整的学习文档了，不需要再初始化。
> 直接说「读档」或「老师我们继续」，我会读 `COACH.md` 和 `progress.md` 接着教。

### 全部不存在

进入初始化流程。

### 部分存在

询问用户：

> 目录里有部分学习文档，但不完整。你想：
> 1. 补全缺失的文件
> 2. 重新初始化（会备份旧文件）
> 3. 我走错目录了

---

## 2. 读取 skill 文件

按顺序读取：

1. `core/onboarding.md` — 初始化流程、问卷、计划生成
2. `core/problem-list.md` — 题单获取方式
3. `core/templates.md` — 文档生成规则
4. `core/coach.md` — 运行时教学规则（要嵌入生成的 `COACH.md`）
5. `core/teaching.md` — 教学流程详解（初始化后不需要，但可参考）
6. `core/update.md` — 更新流程（初始化后不需要）

模板文件在 `assets/`：

- `assets/coach-template.md`
- `assets/progress-template.md`
- `assets/notes-template.md`
- `assets/hot100-list-template.md`
- `assets/hot100-snapshot.md`（题单兜底）

> 路径相对于本 SKILL.md 所在目录。Claude Code 中通常是 `~/.claude/skills/leetcode-hot100-coach/`。
> 如果不确定路径，先问用户，或根据当前加载的 skill 路径推导。

---

## 3. 执行初始化

严格按照 `core/onboarding.md`：

1. 意图确认
2. 获取题单（`core/problem-list.md`）
3. 入学问卷（一次一个问题）
4. 生成个性化计划，和用户确认
5. 生成四个文档 + 平台原生文件
6. 让用户预览
7. 用户确认后开始第一课

---

## 4. 生成文档时的重要规则

- `COACH.md` 必须是**自包含**的：包含学生信息 + `core/coach.md` 的完整内容，不能只写“见 skill 文件”
- 所有 `{{占位符}}` 必须替换
- 平台原生文件（`CLAUDE.md`）写入 `COACH.md` 的**完整内容**，不是指针
- 原生文件头部注明：`本文件由 COACH.md 生成，请优先修改 COACH.md`
- 用户确认计划后才生成文档
- 用户预览确认后才开始教学

---

## 5. 初始化完成后

告诉用户：

> 初始化完成。之后不需要再用 `/leetcode-hot100-coach` 了。
> 直接说「读档」「老师我们继续」或「存档」即可。

---

## 6. 不适用的情况

- 用户只问一道题的解法 → 直接回答，不初始化
- 用户想开发一个项目 → 不初始化
- 用户已经有完整文档 → 短路退出

---

## 7. 平台无关提醒

虽然这是 Claude Code 的适配器，但核心逻辑在 `core/` 中，不要写死 Claude Code 专有工具名。如果需要读写文件，用自然语言描述；如果当前环境不支持某个能力，按 `core/coach.md` 的降级规则处理。
