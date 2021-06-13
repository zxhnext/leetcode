## 题目描述

原题地址：[爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/),解法同[斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

题目难度：简单

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：
```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```
示例 2：
```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

## 思路分析
1. 爬到第三级台阶只能从第二级台阶爬一个台阶，或第一级台阶爬两个台阶。 所以f(3) = f(1) + f(2)
2. 同样的，爬到第四级台阶只能从第三级台阶爬一个台阶，或第二级台阶爬两个台阶 f(4) = f(2) + f(3)
3. ...
4. 爬到第n阶可以在第n-1阶爬1个台阶，或者在第n-2阶爬2个台阶。
5. F(n)= F(n-1) + F(n-2)。

解题步骤：
1. 定义子问题: F(n) = F(n-1) + F(n-2)。
2. 反复执行:从2循环到n，执行上述公式。
### 题解一 动态规划
#### 1. 代码实现
```js
var climbStairs = function(n) {
    if(n < 2) { return 1 }
    const dp = [1, 1];
    for(let i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
};
```
#### 2.空间复杂度优化

```js
var climbStairs = function(n) {
    if(n < 2) { return 1 }
    let dp0 = 1;
    let dp1 = 1;
    for(let i = 2; i <= n; i++) {
        const temp = dp0;
        dp0 = dp1;
        dp1 = dp1 + temp;
    }
    return dp1;
};
```
#### 3. 复杂度分析
时间复杂度O(n), 优化前空间复杂度为O(n), 优化后O(1)

### 题解2 递归法
#### 1. 代码实现
```js
var climbStairs = function(n) {
    if (n < 2) { return 1; }
    return climbStairs(n - 2) + climbStairs(n - 1);
};
```
#### 2. 使用缓存优化
```js
const cache = [];
var climbStairs = function(n) {
    if (n < 2) { return 1; }
    if (cache[n]) return cache[n];
    cache[n] = climbStairs(n - 2) + climbStairs(n - 1);
    return cache[n];
};
```
#### 3. 复杂度分析
时间复杂度 O(2^n)，优化后O(n), 空间复杂度O(1)