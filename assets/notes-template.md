# Hot 100 知识笔记

> 最后更新：{{YYYY-MM-DD}}

---

## 标记说明

- ✅ 已掌握
- ⭐ 需要复习
- ❓ 没懂

---

## 第一部分：核心思路模板

> 按模式分类。每个模板包含：适用场景、核心思路、代码模板、复杂度。

### 1. 哈希表模式

**适用场景：** 需要 O(1) 查找、计数、去重、配对。

**核心思路：**

- 用空间换时间
- 遍历时边查边存
- 注意“先查再存”还是“先存再查”

**代码模板：**

```javascript
const map = new Map();
for (let i = 0; i < nums.length; i++) {
    // 先查
    if (map.has(need)) {
        // 找到答案
    }
    // 再存
    map.set(nums[i], i);
}
```

**复杂度：** 时间 O(n)，空间 O(n)

**典型题：** 1, 128, 560

---

### 2. 双指针模式

**适用场景：** 有序数组、两端向中间、快慢指针、区间问题。

**核心思路：**

- 利用有序性或区间性质，减少重复扫描
- 每步至少排除一个不可能的位置

**代码模板：**

```javascript
let left = 0;
let right = nums.length - 1;
while (left < right) {
    if (满足条件) {
        // 记录答案
        left++;
    } else {
        right--;
    }
}
```

**复杂度：** 时间 O(n)，空间 O(1)

**典型题：** 11, 15, 42

---

### 3. 滑动窗口

**适用场景：** 连续子数组 / 子串，求最长、最短、是否满足条件。

**核心思路：**

- 右指针扩张，左指针收缩
- 维护窗口内的状态（计数、和、最大值）

**代码模板：**

```javascript
let left = 0;
for (let right = 0; right < n; right++) {
    // 加入 right
    while (不满足条件) {
        // 移除 left
        left++;
    }
    // 更新答案
}
```

**复杂度：** 时间 O(n)，空间 O(k)

**典型题：** 3, 438, 239

---

### 4. 链表模式

**适用场景：** 链表遍历、反转、合并、找环、删除节点。

**核心思路：**

- 哑节点处理头节点
- 快慢指针找中点 / 找环
- 反转时先存 next，再改指向

**代码模板：**

```javascript
const dummy = new ListNode(0);
dummy.next = head;
let prev = dummy;
let curr = head;
while (curr) {
    // ...
}
return dummy.next;
```

**复杂度：** 时间 O(n)，空间 O(1)

**典型题：** 206, 142, 146

---

### 5. 栈与队列

**适用场景：** 括号匹配、嵌套结构、单调性、滑动窗口最大值。

**核心思路：**

- 栈：后进先出，处理最近的相关元素
- 单调栈：弹出破坏单调性的元素，在弹出时结算
- 队列：先进先出，BFS / 滑动窗口

**代码模板（单调栈）：**

```javascript
const stack = [];
for (let i = 0; i < n; i++) {
    while (stack.length && nums[stack[stack.length - 1]] < nums[i]) {
        const idx = stack.pop();
        // 结算 idx
    }
    stack.push(i);
}
```

**复杂度：** 时间 O(n)，空间 O(n)

**典型题：** 20, 155, 739

---

### 6. 二叉树

**适用场景：** 遍历、深度、路径、构造、BST、LCA。

**核心思路：**

- 递归定义：当前节点 + 左右子树
- 明确递归函数的返回值含义
- 先序 / 中序 / 后序 / 层序

**代码模板（DFS）：**

```javascript
function dfs(node) {
    if (node === null) return;
    // 先序：处理 node
    dfs(node.left);
    // 中序：处理 node
    dfs(node.right);
    // 后序：处理 node
}
```

**复杂度：** 时间 O(n)，空间 O(h)

**典型题：** 94, 102, 104, 226, 101

---

### 7. 图

**适用场景：** 岛屿、连通性、拓扑排序、最短路径。

**核心思路：**

- DFS / BFS 遍历
- 访问标记避免重复
- 拓扑排序：入度 + 队列

**代码模板（网格 DFS）：**

```javascript
function dfs(r, c) {
    if (越界 || 已访问 || 不是目标) return;
    visited[r][c] = true;
    dfs(r + 1, c);
    dfs(r - 1, c);
    dfs(r, c + 1);
    dfs(r, c - 1);
}
```

**复杂度：** 时间 O(mn)，空间 O(mn)

**典型题：** 200, 207, 208

---

### 8. 动态规划

**适用场景：** 最优值、计数、可行性，且满足最优子结构 + 重叠子问题。

**核心思路：**

1. 定义状态：`dp[i]` 表示什么
2. 找转移方程
3. 初始化
4. 遍历顺序
5. 返回结果

**代码模板（一维 DP）：**

```javascript
const dp = new Array(n + 1).fill(0);
dp[0] = 初始值;
for (let i = 1; i <= n; i++) {
    dp[i] = 由 dp[i-1] / dp[i-2] 推导;
}
return dp[n];
```

**复杂度：** 时间 O(n) 或 O(n²)，空间 O(n) 或可优化到 O(1)

**典型题：** 70, 198, 300

---

## 第二部分：JavaScript 基础

> 遇到再补，不追求系统完备。

### Map

```javascript
const map = new Map();
map.set(key, value);
map.get(key);
map.has(key);
map.delete(key);
map.size;
```

### Set

```javascript
const set = new Set();
set.add(value);
set.has(value);
set.delete(value);
set.size;
```

### 数组常用方法

```javascript
arr.push(x);        // 尾部添加
arr.pop();          // 尾部删除
arr.shift();        // 头部删除（O(n)，少用）
arr.unshift(x);     // 头部添加（O(n)，少用）
arr.slice(start, end);  // 切片，不修改原数组
arr.splice(start, count, ...items); // 修改原数组
arr.sort((a, b) => a - b); // 排序
arr.reverse();      // 反转
```

### 字符串

```javascript
str.split('');      // 转数组
arr.join('');       // 转字符串
str.charAt(i);      // 第 i 个字符
str.charCodeAt(i);  // 字符编码
str.substring(start, end);
str.slice(start, end);
```

### 循环

```javascript
for (let i = 0; i < n; i++) {}
for (const x of arr) {}
for (const key in obj) {}
while (condition) {}
```

---

## 第三部分：题目详解

> 每完成一道题，按下面格式追加。

### {{题号}}. {{题名}}

**难度：** {{简单 / 中等 / 困难}}  
**完成日期：** {{YYYY-MM-DD}}  
**思想标签：** {{哈希 / 双指针 / DP / ...}}

**题目：** {{一句话描述}}

**核心思路：**

- {{关键洞察}}
- {{步骤}}

**代码：**

```javascript
{{代码}}
```

**时间复杂度：** {{O(...)}}  
**空间复杂度：** {{O(...)}}

**易错点：**

- {{易错点}}

**学习心得：**

- {{心得}}

---

**创建日期：** {{YYYY-MM-DD}}  
**最后更新：** {{YYYY-MM-DD}}
