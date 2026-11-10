# leetcode-hot100-coach

> 一个平台无关的 LeetCode Hot 100 手写训练教练  
> 让 AI 像老师一样带你刷题，而不是直接把答案塞给你。

---

## 为什么做这个

- 直接问 AI，答案来得太快，看完就忘
- Hot 100 题单会变，网上的学习计划经常过期
- 面试要手写，**看懂了 ≠ 写得出**
- 每个 AI 平台都要重新配置一套提示词，太麻烦

这个 skill 把「教、练、存档、复习」整理成一套可复用的流程，并且**不绑定任何特定 AI 平台**。

---

## 特点

- **平台无关**：Claude Code / Codex / Cursor / DeepSeek / ChatGPT / Claude.ai 都能用
- **语言无关**：JavaScript / Python / Java / C++ / Go 等，代码和笔记按你选的语言生成
- **只初始化一次**：生成学习文档后，后续由 `COACH.md` 驱动，不再加载 skill
- **题单实时获取**：优先从 LeetCode CN 抓取，失败用内置快照兜底
- **计划个性化**：根据基础、每天时间、目标日期生成，不套固定 8 周
- **两种模式**：手写模式（默认）/ 只讲思路模式
- **存档 / 读档**：跨对话恢复进度
- **自动 git commit**：检测到 git 仓库时，存档自动提交
- **三层知识笔记**：核心模板 / 语言基础 / 题目详解

---

## 支持平台

| 平台 | 入口文件 | 放置位置 |
|------|----------|----------|
| Claude Code | `SKILL.md`（仓库根目录） | `~/.claude/skills/leetcode-hot100-coach/` |
| Codex | `adapters/codex/AGENTS.md` | 学习项目根目录 |
| Cursor | `adapters/cursor/.cursorrules` | 学习项目根目录 |
| 通用聊天模型 | `adapters/generic/PROMPT.md` | 粘贴到对话或自定义指令 |

---

## 安装

### Claude Code

```bash
git clone https://github.com/0124draky-arch/leetcode-hot100-coach ~/.claude/skills/leetcode-hot100-coach
```

> 克隆后仓库根目录就是 skill 目录，Claude Code 会自动读取根目录的 `SKILL.md`。

然后在一个**新的学习目录**里触发：

```
/leetcode-hot100-coach
```

或直接说：

```
初始化 Hot 100 教练
```

### Codex

```bash
git clone https://github.com/0124draky-arch/leetcode-hot100-coach ~/tools/leetcode-hot100-coach
cp ~/tools/leetcode-hot100-coach/adapters/codex/AGENTS.md ./AGENTS.md
```

打开 `AGENTS.md`，把 `{{SKILL_DIR}}` 替换成你的实际路径（例如 `/Users/you/tools/leetcode-hot100-coach`），然后在 Codex 中开始对话。

### Cursor

```bash
cp ~/tools/leetcode-hot100-coach/adapters/cursor/.cursorrules ./
```

同样把 `{{SKILL_DIR}}` 替换成实际路径。

### 通用聊天模型（DeepSeek / ChatGPT / Claude.ai）

1. 打开 `adapters/generic/PROMPT.md`
2. 全文粘贴到对话开头，或设置为自定义指令 / 项目知识
3. 按提示完成问卷

> 聊天模型没有文件读写能力，初始化后的文档需要你自己保存；每次新对话把 `COACH.md`、`progress.md`、`learning-notes.md` 贴回去即可。

---

## 使用

### 基本操作（只需要记住 4 个）

| 操作 | 你可以说 | 作用 |
|------|----------|------|
| 初始化 | `/leetcode-hot100-coach` 或 `初始化 Hot 100 教练` | 只做一次 |
| 继续学习 | `读档` / `老师我们继续` | 新对话恢复进度 |
| 存档 | `存档` | 保存进度；有 git 则自动提交 |
| 复习 | `复习薄弱点` | 按笔记中的 ⭐ 出题 |

### 使用场景

#### 场景 1：第一次使用

```
你：/leetcode-hot100-coach

AI：你想系统刷 Hot 100。先问几个问题：
    你做过多少题？主要会哪些专题？用什么语言？

你：做过 30 题，会数组和链表，用 JavaScript。

AI：（继续问目标、时间、难度、模式）
    建议 10 周，每天 2 题。确认后我会生成 COACH.md、progress.md 等文件。
```

#### 场景 2：第二天继续

```
你：读档

AI：上次学到第 1 周，完成了 1、169、448。
    薄弱点：动态规划、图。
    今天继续 49 和 128，还是先复习 5 分钟？
```

#### 场景 3：学完一题

```
你：存档

AI：已更新 progress.md 和 learning-notes.md。
    今天完成 1、169；下次从 448 开始。
    （如果目录是 git 仓库，已自动 commit）
```

#### 场景 4：周末复习

```
你：复习薄弱点

AI：从 ⭐ 里出 3 个问题：
    1. 哈希表为什么能做到 O(1) 查找？
    2. 滑动窗口什么时候移动左指针？
    3. 二叉树递归的出口是什么？
```

### 维护操作（用到再说）

| 操作 | 你可以说 | 作用 |
|------|----------|------|
| 更新题单 | `更新题单` | 抓取或粘贴最新题单，显示差异后更新 |
| 调整计划 | `太快了` / `每天没时间` | 调整周期和每天题量 |
| 切换模式 | `切换成只讲思路模式` | 手写 ↔ 只讲思路 |
| 调整难度 | `困难题先跳过` | 调整难度范围和题量 |
| 换语言 | `换成 Python` | 重写代码模板和语言笔记 |
| 换目录 | `换个目录` | 迁移学习文档 |
| 重新初始化 | `全部重来` | 备份后重新制定计划 |

> 完整指令表见 [core/commands.md](core/commands.md)。

---

## 初始化后会生成什么

在你的学习目录中生成：

```
COACH.md            # 完整运行时规则 + 学生信息（源文件）
progress.md         # 个性化计划 + 当前进度 + 已完成题目
learning-notes.md   # 三层知识笔记
hot100-list.md      # 本次使用的题单快照（带日期）

CLAUDE.md           # Claude Code：COACH.md 全文（自动加载）
AGENTS.md           # Codex：COACH.md 全文（自动加载）
.cursorrules        # Cursor：COACH.md 全文（自动加载）
```

- `COACH.md` 是**源文件**，唯一事实来源
- 平台原生文件是 `COACH.md` 的**完整副本**，不是指针；只生成当前平台的
- 修改 `COACH.md` 后，重新生成对应的原生文件，避免漂移
- 纯聊天模型不用生成原生文件，直接使用 `COACH.md`

---

## 题单来源

以 **https://leetcode.cn/problem-list/2cktkvj/** 为准。

初始化时按以下优先级获取：

1. 用户自己提供的题单
2. LeetCode CN GraphQL 接口（实时）
3. 内置快照 `assets/hot100-snapshot.md`（带日期）
4. 请用户粘贴

每次初始化都会把本次使用的题单写入 `hot100-list.md`，方便以后追溯。

---

## 项目结构

```
leetcode-hot100-coach/
├── SKILL.md                    # Claude Code 入口（根目录）
├── README.md
├── LICENSE
├── core/                       # 平台无关的核心规则
│   ├── coach.md                # 教学主规则
│   ├── onboarding.md           # 初始化流程 + 问卷 + 计划生成
│   ├── problem-list.md         # 题单获取 / 校验 / 兜底
│   ├── templates.md            # 生成文档的模板
│   ├── teaching.md             # 教学流程详解
│   ├── update.md               # 更新题单 / 调整计划 / 切换模式 / 换语言 / 迁移
│   └── commands.md             # 完整指令清单（单一来源）
├── adapters/                   # 其他平台薄适配层
│   ├── codex/AGENTS.md
│   ├── cursor/.cursorrules
│   └── generic/PROMPT.md
├── assets/
│   ├── hot100-snapshot.md      # 题单快照（兜底，Markdown）
│   ├── hot100-snapshot.json    # 题单快照（兜底，JSON）
│   ├── coach-template.md       # 运行时 COACH.md 模板
│   ├── progress-template.md    # 进度模板
│   ├── notes-template.md       # 知识笔记模板
│   └── hot100-list-template.md # 题单文件模板
└── examples/
    ├── walkthrough-206.md      # 206 反转链表完整带做示例
    ├── 8-week-plan.md          # 8 周节奏参考样例（不是默认值）
    ├── demo-output/            # 有基础学习者的初始化试跑
    └── demo-output-beginner/   # 零基础学习者的初始化试跑
```

---

## 设计原则

1. **一份核心规则，多个平台适配**：`core/` 是唯一事实来源；初始化时把完整规则写入平台原生文件
2. **初始化只跑一次**：之后由用户目录里的 `COACH.md` 驱动
3. **不写死工具名**：用「读取文件 / 写入文件 / 如果支持 git」这类平台无关表述
4. **能力降级**：没有文件读写 → 聊天模式；没有网络 → 快照；没有 git → 跳过提交
5. **计划是生成出来的**：8 周只是参考样例，不是默认值

---

## 常见问题

### 更新题单时提示网络被拦截？

Claude Code 的沙箱或权限设置可能禁止联网。skill 会自动切换到手动模式：

1. 打开 https://leetcode.cn/problem-list/2cktkvj/
2. 复制题单内容，或整理成题号列表
3. 粘贴给 AI，说“用这个更新题单”

不需要反复重试网络请求，手动提供题单同样可以更新。

### `/leetcode-hot100-coach` 没有反应？

- 确认 `~/.claude/skills/leetcode-hot100-coach/SKILL.md` 存在
- 确认 `SKILL.md` 开头的 frontmatter 有 `name: leetcode-hot100-coach`
- 重启 Claude Code 或新开一个会话
- 确认当前目录不是 skill 目录本身

---

## Roadmap

- [ ] 补一份英文完整 README
- [ ] 增加 VS Code Copilot / Windsurf 适配器
- [ ] 增加「只看思想」模式的更多示例
- [ ] 增加题单更新差异对比的截图
- [ ] 增加学习报告生成（阶段性总结）

---

## License

MIT

---

## English Summary

**leetcode-hot100-coach** is a platform-agnostic AI coaching skill for learning LeetCode Hot 100 by hand.

It initializes a learning project once, generating `COACH.md`, `progress.md`, `learning-notes.md`, and a dated problem-list snapshot. After that, every session is driven by those files — no need to reload the skill.

Works with Claude Code, Codex, Cursor, and any chat model that can follow Markdown instructions. It is language-agnostic: JavaScript, Python, Java, C++, Go, and others are supported by adapting the code templates and notes to the learner's language. The problem list is fetched from LeetCode CN at initialization, with a bundled snapshot as fallback.

Key ideas: interactive teaching, tiered hints, handwriting practice, personalized schedule, archive/restore across sessions, and optional git auto-commit.
