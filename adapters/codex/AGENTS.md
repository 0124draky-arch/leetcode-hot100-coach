# Hot 100 手写教练（Codex 入口）

> 使用前把 `{{SKILL_DIR}}` 替换成你本机 skill 仓库的实际路径，例如
> `/Users/you/tools/leetcode-hot100-coach`。
>
> **注意：** 初始化完成后，本文件会被 `COACH.md` 的完整内容覆盖。
> 这是预期行为；之后本文件就是完整规则，不再是指针。

---

## 每次对话开始时

### 情况 A：当前目录已有 `COACH.md`

1. 读取 `COACH.md`
2. 读取 `progress.md`
3. 读取 `learning-notes.md`
4. 严格按 `COACH.md` 的规则继续教学
5. **不要**重新初始化，**不要**再读 skill 目录

### 情况 B：当前目录没有 `COACH.md`

说明还没有初始化。读取以下文件并执行初始化：

- `{{SKILL_DIR}}/core/onboarding.md`
- `{{SKILL_DIR}}/core/problem-list.md`
- `{{SKILL_DIR}}/core/templates.md`
- `{{SKILL_DIR}}/core/coach.md`
- `{{SKILL_DIR}}/assets/coach-template.md`
- `{{SKILL_DIR}}/assets/progress-template.md`
- `{{SKILL_DIR}}/assets/notes-template.md`
- `{{SKILL_DIR}}/assets/hot100-list-template.md`
- `{{SKILL_DIR}}/assets/hot100-snapshot.md`

如果这些文件不存在，告诉用户：

> 我找不到 skill 文件。请确认 `AGENTS.md` 里的 `{{SKILL_DIR}}` 路径是否正确，
> 或者把 `leetcode-hot100-coach` 仓库克隆到本机，并把路径填进来。

---

## 初始化要点

1. 确认用户真的想系统学 Hot 100
2. 获取当前题单（优先 LeetCode CN GraphQL，失败用快照）
3. 一次问一个问题，完成入学问卷
4. 根据基础和每天时间生成计划，和用户确认
5. 生成：
   - `COACH.md`
   - `progress.md`
   - `learning-notes.md`
   - `hot100-list.md`
6. 让用户预览，确认后开始第一课

**`COACH.md` 必须自包含**：包含学生信息 + 完整教学规则，不能只写“见 skill 文件”。

---

## 初始化完成后

初始化流程会**用 `COACH.md` 的完整内容覆盖本文件（`AGENTS.md`）**，并在头部加生成说明。
所以初始化完成后，本文件本身就是完整规则，不再是指针。

告诉用户：

> 初始化完成。之后每次对话我都会读本文件（`AGENTS.md`）和 `progress.md`，不需要再初始化。
> 直接说「读档」「存档」或「老师我们继续」即可。

---

## 能力降级

- 不能联网 → 用 `assets/hot100-snapshot.md`
- 不能写文件 → 把四个文件内容用代码块输出，让用户自己保存
- 不能执行 git → 跳过自动提交，只更新文件
