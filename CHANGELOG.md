# Changelog

本项目遵循 [Semantic Versioning](https://semver.org/)。

## v0.1.1 — 2026-11-10

首个公开版本。

### 新增

- 平台无关的 LeetCode Hot 100 手写教练 Skill
- 支持 Claude Code、Codex、Cursor、DSH（DeepSeek Harness）、通用聊天模型
- 初始化：入学问卷、个性化周期、专题分组排周、困难题标记
- 题单：LeetCode CN GraphQL 实时获取，网络被拦截时手动粘贴兜底
- 教学：手写模式 / 只讲思路模式、提示分级、代码审查、手动模拟
- 存档 / 读档 / 复习
- 维护：更新题单、调整计划、切换模式、调整难度、换语言、迁移、重新初始化
- 语言无关：JavaScript / Python / Java / C++ / Go 等
- 完整指令清单 `core/commands.md`
- 两份初始化试跑示例：有基础版、零基础版

### 修复

- 将 Claude Code 的 `SKILL.md` 移到仓库根目录，修复无法加载的问题
- 更新题单在网络被拦截时不再反复重试，直接进入手动模式
- DSH 初始化时生成项目根目录 `AGENTS.md` 指针，确保新对话自动读取 `COACH.md`

### 验证

- **Claude Code**：初始化与触发已实测
- **DSH**：Skill 加载、`AGENTS.md` 注入、新对话读档已实测
- **Codex / Cursor / 通用聊天**：适配器已提供，尚未完整实测

### 已知限制

- Codex / Cursor / 通用聊天适配器尚未完整实测
- 网络被拦截时，更新题单需要用户手动粘贴题单
- DSH 使用单文件 Skill 版，和仓库 `core/` 需要手动同步
- 困难题默认选做，不强制完成
