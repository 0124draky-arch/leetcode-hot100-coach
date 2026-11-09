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
- **只初始化一次**：生成学习文档后，后续由 `COACH.md` 驱动，不再加载 skill
- **题单实时获取**：优先从 LeetCode CN 抓取，失败用内置快照兜底
- **计划个性化**：根据基础、每天时间、目标日期生成，不套固定 8 周
- **两种模式**：手写模式（默认）/ 只讲思路模式
- **存档 / 读档**：跨对话恢复进度
- **自动 git commit**：检测到 git 仓库时，存档自动提交
- **三层知识笔记**：核心模板 / JavaScript 基础 / 题目详解

---

## 支持平台

| 平台 | 入口文件 | 放置位置 |
|------|----------|----------|
| Claude Code | `adapters/claude-code/SKILL.md` | `~/.claude/skills/leetcode-hot100-coach/` |
| Codex | `adapters/codex/AGENTS.md` | 学习项目根目录 |
| Cursor | `adapters/cursor/.cursorrules` | 学习项目根目录 |
| 通用聊天模型 | `adapters/generic/PROMPT.md` | 粘贴到对话或自定义指令 |

---

## 安装

### Claude Code

```bash
git clone https://github.com/YOUR_GITHUB_USERNAME/leetcode-hot100-coach ~/.claude/skills/leetcode-hot100-coach
```

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
git clone https://github.com/YOUR_GITHUB_USERNAME/leetcode-hot100-coach ~/tools/leetcode-hot100-coach
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

| 操作 | 你可以说 | 说明 |
|------|----------|------|
| 初始化 | `初始化 Hot 100 教练` / `/leetcode-hot100-coach` | 只需一次 |
| 继续学习 | `读档` / `老师我们继续` | 新对话恢复进度 |
| 存档 | `存档` | 更新文档；有 git 则自动提交 |
| 更新题单 | `更新题单` | 重新抓取并显示差异 |
| 切换模式 | `切换成只讲思路模式` | 修改 `COACH.md` |
| 复习 | `复习薄弱点` | 按笔记中的 ⭐ 出题 |

---

## 初始化后会生成什么

在你的学习目录中生成：

```
COACH.md            # 完整运行时规则 + 学生信息（唯一事实来源）
progress.md         # 个性化计划 + 当前进度 + 已完成题目
learning-notes.md   # 三层知识笔记
hot100-list.md      # 本次使用的题单快照（带日期）

CLAUDE.md           # 指针：读取 COACH.md 并遵循
AGENTS.md           # 指针：读取 COACH.md 并遵循
```

- `COACH.md` 是唯一事实来源，后续教学只看它
- `CLAUDE.md` / `AGENTS.md` 只做指针，避免多份规则漂移
- 纯聊天模型不用生成指针文件，直接使用 `COACH.md`

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
├── README.md
├── LICENSE
├── core/                       # 平台无关的核心规则
│   ├── coach.md                # 教学主规则
│   ├── onboarding.md           # 初始化流程 + 问卷 + 计划生成
│   ├── problem-list.md         # 题单获取 / 校验 / 兜底
│   ├── templates.md            # 生成文档的模板
│   ├── teaching.md             # 教学流程详解
│   └── update.md               # 更新题单 / 调整计划 / 切换模式
├── adapters/                   # 各平台薄适配层
│   ├── claude-code/SKILL.md
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
    └── 8-week-plan.md          # 8 周节奏参考样例（不是默认值）
```

---

## 设计原则

1. **一份核心规则，多个平台适配**：`core/` 是唯一事实来源
2. **初始化只跑一次**：之后由用户目录里的 `COACH.md` 驱动
3. **不写死工具名**：用「读取文件 / 写入文件 / 如果支持 git」这类平台无关表述
4. **能力降级**：没有文件读写 → 聊天模式；没有网络 → 快照；没有 git → 跳过提交
5. **计划是生成出来的**：8 周只是参考样例，不是默认值

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

Works with Claude Code, Codex, Cursor, and any chat model that can follow Markdown instructions. The problem list is fetched from LeetCode CN at initialization, with a bundled snapshot as fallback.

Key ideas: interactive teaching, tiered hints, handwriting practice, personalized schedule, archive/restore across sessions, and optional git auto-commit.
