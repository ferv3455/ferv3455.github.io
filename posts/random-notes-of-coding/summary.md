---
layout: post
title: Random Notes of Coding Summary
date: 2024/9/19
---

# 解题思路总结

## 数组

- 二分查找：适用于快速缩小范围。可调整返回值和分支条件来实现不同目标
- 双指针：起始位置、移动速度、终止条件
- 滑动窗口：使用前提为不存在两个局部最优解，其中一个被另一个真包含。先扩展到符合要求，再收缩直到不符合，最后比较。需要维护窗口信息（如上一次出现元素的位置、最大最小值、已经匹配条件的位置数等）
- 子序列的和可以等价于前缀和之差，用于动态规划
- 牛顿迭代法：适用于求函数的近似零点，用切线逼近

## 链表

- 辅助节点
- 通常可用递归和迭代两种方式求解
- 双指针：起始位置、移动速度、终止条件。有时需要利用链表中节点的距离关系不变性

## 哈希表

- 可用作计数器、visited 集合、反向索引
- 键较简单时（字符），可以使用数组来替代，取得更高效率
- 如果只需要 0-1 计数器，则可退化为集合（判断重复）
- n 数之和：反向索引、预排序+双指针、迭代确定起点+跳过元素去重
- 桶排序

## 字符串

- KMP：考察前缀与后缀相等的长度
- 重复子串问题：尝试在末尾拼接原字符串
- 回文串问题：如果不修改原字符串，则可以动态规划判断所有子串是否为回文串；如果要修改原字符串，则可以尝试直接比较，处理异常位置
- 可以使用 trie 来加快匹配速度

```c++
struct Trie {
    bool terminal = false;
    unordered_map<char, Trie *> children;

    void addPath(const string &word) {
        Trie *tmp = this;
        for (const char c : word) {
            if (!tmp->children.count(c))
                tmp->children[c] = new Trie();
            tmp = tmp->children[c];
        }
        tmp->terminal = true;
    }
};
```

- 字符串的暴力匹配/搜索有时并不会带来较大的开销

### 栈与队列

- 可用的数据结构：栈、队列、双端队列（滑动窗口最值）、优先队列
- 最大的 k 个元素：小顶堆、快速切分

## 二叉树

- 前序遍历、后序遍历、中序遍历：都基于 DFS 思路实现，使用栈进行迭代
    - 通用迭代方式：标记父节点
    - 前序遍历迭代：正常顺序的入栈（遍历问题的常见解答）
    - 中序遍历迭代：先沿左侧往下依次入栈，弹出栈顶并处理，移到右子树
- 层序遍历：BFS 两层循环，内循环使用队列的长度。可以通过控制入队顺序来实现功能
- 递归：自顶向下（传参数），自底向上（返回值）
- 二叉树中每个节点的位置可以用一个二进制数表示（Huffman）
- 使用哈希表构造指向父节点的反向链表
- 二叉树的构造：可以使用队列、栈（单调栈）等结构来辅助，维护顺序
- 在遍历中加入 NULL 即可使前序后序遍历结果与树一一对应

## 回溯算法

- 通用解答：当前路径记录+递归，终止条件+剪枝+选不选当前元素
- 全子集问题：使用二进制位向量表示
- 去重：主动查找（不选时跳过相等元素），被动判断（如果之前选择了相等元素则必须选择）
- 元素无序时需要预排序才可以去重
- 结果有序时需要添加额外的 visited 数组（可用位向量替代）来追踪路径

## 贪心算法

- 区间是连续变化的，只需考虑最远位置即可
- 排序+依次按贪心原则分配
- 负和清零：累计和为负时，表明以此为终点的子序和都为负，从零开始
- 重叠区间：排序后，每次先选择当前位置，后续的位置有添加/替换上一个两种选择

## 单调栈

- 单调栈的选择：单调性、原数组迭代方向、存储索引/值
- 核心思路：已被弹出的元素不会再被使用到（大小关系被覆盖）
- 可以在入栈或出栈时更新结果，需要的迭代方向相反
- 同时要求左右两侧的最大值时（接雨水）无需单调栈（可用双指针简化）
- 同时要求左右两侧的下一个更大值时，可以使用一次遍历解决（条件略有差异）

## 动态规划

- 找子问题（递推公式）
- 滚动数组优化：减小空间复杂度。对于一般的动态规划问题，分析 dp 数组的依赖关系后，通常可以通过调整运算顺序的方式优化空间
- 0-1 背包/完全背包：二维 dp 数组（重量、物品编号，内容为最大价值），可优化为一维数组。0-1 背包需要倒序遍历，完全背包需要正序遍历
- 有选择顺序的完全背包：交换内外层迭代顺序
- 正负号反转/选择的求和问题可以转化为子集求和问题
- 摆动序列/股票问题：讨论所有可能的状态，分别维护各自的 dp 数组，递推求解
- 回文串：动态规划 dp 仅取决于对角线元素，可滚动优化