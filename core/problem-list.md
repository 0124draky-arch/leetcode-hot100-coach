# 题单获取与校验

> 题单会变。**以 LeetCode CN 的 Hot 100 题单为准**，不要使用记忆中的旧列表。

---

## 1. 唯一来源

**题单页面：** https://leetcode.cn/problem-list/2cktkvj/

注意：这个页面是前端渲染的，直接抓 HTML 可能拿到 404 或空数据。**优先使用 GraphQL 接口。**

---

## 2. 获取优先级

| 优先级 | 来源 | 说明 |
|--------|------|------|
| 1 | 用户提供的题单 | 用户粘贴或指定文件时，优先使用 |
| 2 | LeetCode CN GraphQL | 实时获取，最准确 |
| 3 | `assets/hot100-snapshot.md` | 内置快照，带日期；可能过期 |
| 4 | 请用户粘贴 | 无网络、无快照时 |

---

## 3. GraphQL 获取方式

### 请求

```
POST https://leetcode.cn/graphql/
Content-Type: application/json
Referer: https://leetcode.cn/problem-list/2cktkvj/
User-Agent: Mozilla/5.0
```

### Query

```graphql
query favoriteQuestionList($favoriteSlug: String!) {
  favoriteQuestionList(favoriteSlug: $favoriteSlug) {
    questions {
      questionFrontendId
      title
      translatedTitle
      titleSlug
      difficulty
      topicTags {
        name
        slug
        nameTranslated
      }
    }
  }
}
```

### Variables

```json
{ "favoriteSlug": "2cktkvj" }
```

### 返回示例

```json
{
  "data": {
    "favoriteQuestionList": {
      "questions": [
        {
          "questionFrontendId": "160",
          "translatedTitle": "相交链表",
          "titleSlug": "intersection-of-two-linked-lists",
          "difficulty": "EASY",
          "topicTags": [
            { "name": "Hash Table", "slug": "hash-table", "nameTranslated": "哈希表" },
            { "name": "Linked List", "slug": "linked-list", "nameTranslated": "链表" },
            { "name": "Two Pointers", "slug": "two-pointers", "nameTranslated": "双指针" }
          ]
        }
      ]
    }
  }
}
```

### 难度映射

| API 值 | 中文 |
|--------|------|
| `EASY` | 简单 |
| `MEDIUM` | 中等 |
| `HARD` | 困难 |

### 标签的用途

`topicTags` 用于生成个性化计划时的**主题分组**：

- 每道题按标签归入一个主专题
- 分组规则和优先级见 `core/onboarding.md` 的「主题分组与排周」
- 没有标签时，退化用题目标题和用户偏好分组

---

## 4. 获取后的处理

1. 按题号升序排序
2. 统计难度分布
3. 写入 `hot100-list.md`，格式见 `assets/hot100-list-template.md`
4. 在文件头记录：
   - 来源 URL
   - 获取日期（YYYY-MM-DD）
   - 题目总数
   - 难度分布

**写入前先告诉用户：**

> 已获取当前 Hot 100 题单：共 100 题，简单 21 / 中等 66 / 困难 13（日期：YYYY-MM-DD）。

---

## 5. 获取失败时

### 有快照

使用 `assets/hot100-snapshot.md`，并告诉用户：

> 当前无法访问 LeetCode，我使用了内置题单快照（日期：YYYY-MM-DD）。
> 如果题单有变化，你可以把最新题单粘贴给我，我以你的为准。

### 没有快照

请用户提供：

> 我暂时拿不到最新题单。你可以：
> 1. 把 LeetCode 题单页面复制粘贴给我
> 2. 或者提供题号列表（例如：1, 2, 3, ...）

---

## 6. 更新题单

用户说“更新题单”时：

1. 重新获取最新题单
2. 和当前 `hot100-list.md` 对比
3. 显示差异：
   - 新增题目
   - 移除题目
   - 难度变化
4. 询问用户：
   > 题单有变化：新增 X 题，移除 Y 题。要更新 `hot100-list.md` 和 `progress.md` 的计划吗？
5. 用户确认后：
   - 更新 `hot100-list.md`
   - 调整 `progress.md` 中的计划
   - 已完成题目不受影响
   - 如果目录是 git 仓库，自动 commit

**不要在没有确认的情况下直接替换题单。**

---

## 7. 纯聊天模型

如果平台不能联网、不能写文件：

1. 让用户粘贴题单
2. AI 整理成 Markdown 表格
3. 用户自己保存为 `hot100-list.md`
4. 后续每次新对话，用户需要把题单一起贴回来（或只贴当前进度所需的部分）

---

## 8. 快照维护

- 内置快照放在 `assets/hot100-snapshot.md`
- 每次大版本更新时，重新抓取并覆盖快照
- 快照头部必须写明日期和来源
- 如果快照超过 90 天，初始化时优先尝试实时获取，并提示用户快照可能过期

---

## 9. 题单文件格式

见 `assets/hot100-list-template.md`。

核心字段：

| 字段 | 说明 |
|------|------|
| 题号 | LeetCode 题号 |
| 中文标题 | `translatedTitle` |
| 难度 | 简单 / 中等 / 困难 |
| slug | `titleSlug`，用于生成链接 |
| 状态 | 未做 / 已完成 / 需复习 |
| 备注 | 一句话思路或薄弱点 |
