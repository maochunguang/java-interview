# 动态规划
## 什么是动态规划？
用一句话解释动态规划就是 “记住你之前做过的事”，如果更准确些，其实是 “记住你之前得到的答案”。

## 动态规划解法
动态规划的的四个解题步骤是：
1. 定义子问题
2. 写出子问题的递推关系
3. 确定 DP 数组的计算顺序
4. 空间优化（可选）
不瞒你说，在 LeetCode 上我做过的几十道动态规划题目，都是用这个解题四步骤做出来的。这个解题步骤适用于任何一道动态规划题目，能够让你很快理清解题的各个要点。

## 常见动态规划题

### 1、爬楼梯：
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数
```
示例一：
输入：2
输出：2
解释： 有两种方法可以爬到楼顶。

1. 1 阶 + 1 阶
2. 2 阶

示例二：
输入：3
输出：3
解释： 有三种方法可以爬到楼顶。

1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```
### 2、三角形最小路径和：
给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。
例如，给定三角形：
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
说明：如果你可以只使用 O(n) 的额外空间（n 为三角形的总行数）来解决这个问题，那么你的算法会很加分。

### 3、最大连续子序和
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
```
示例:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6
```
进阶：如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

### 4、最长公共子序列(LeetCode 1143)
```
给定两个字符串 s 和 t，返回这两个字符串的最长公共子序列的长度。若这两个字符串没有公共子序列，则返回 0。
一个字符串的子序列是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。例如，"ace" 是 "abcde" 的子序列。
```
### 5、打家劫舍（LeetCode 198）
>你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。
```
输入: [1,2,3,1]
输出: 4
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

### 6、打家劫舍2（LeetCode 213）
>你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都围成一圈，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。
```
示例 1:

输入: [2,3,2]
输出: 3
解释: 你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。

示例 2:

输入: [1,2,3,1]
输出: 4
解释: 你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

### 7、整数拆分（LeetCode 343）
给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。说明: 你可以假设 n 不小于 2 且不大于 58。
```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。

示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```

### 8、编辑距离（LeetCode 72）
> 给定字符串 s 和 t，将 s 转换成 t。你可以进行三种操作：插入一个字符、删除一个字符、替换一个字符。计算将 s 将转换成 t 的最小操作数。
```
示例：
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
    horse -> rorse (将 'h' 替换为 'r')
    rorse -> rose (删除 'r')
    rose -> ros (删除 'e')
```