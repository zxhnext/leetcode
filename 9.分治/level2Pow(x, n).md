## 题目描述

原题地址：[Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

**难度：中等**

实现 pow(x, n) ，即计算 x 的 n 次幂函数（即，x^n）。

示例 1：
```
输入：x = 2.00000, n = 10
输出：1024.00000
```
示例 2：
```
输入：x = 2.10000, n = 3
输出：9.26100
```
示例 3：
```
输入：x = 2.00000, n = -2
输出：0.25000
解释：2^-2 = 1/2^2 = 1/4 = 0.25
```

提示：

-100.0 < x < 100.0
-2^31 <= n <= 2^31-1
-10^4 <= x^n <= 10^4

## 题解
使用二分法

#### 1. 问题分析
1. 分：将2^n转为 (2^ 2/n) * (2^ 2/n)
2. 解：求2^2/n
3. 合：(2^ 2/n) * (2^ 2/n)

#### 2. 代码实现
```js
function quickMul (x, n) {
    if (n === 0) {
        return 1;
    }
    const y = quickMul(x, Math.floor(n / 2));
    return n % 2 === 0 ? y * y : y * y * x;
}
```

#### 3.  复杂度度分析
时间复杂度O(logn),空间复杂度O(logn)

牛顿迭代法原理 http://www.matrix67.com/blog/archives/361  
牛顿迭代法代码 http://www.voidcn.com/article/p-eudisdmk-zm.html 