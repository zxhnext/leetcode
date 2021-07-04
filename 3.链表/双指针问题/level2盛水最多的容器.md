## 题目描述

原题地址：[盛水最多的容器](https://leetcode-cn.com/problems/3sum/)

难度：中等

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器。

示例 1：
![](./img/question_11.jpeg)
```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```
示例 2：
```
输入：height = [1,1]
输出：1
```
示例 3：
```
输入：height = [4,3,2,1,4]
输出：16
```
示例 4：
```
输入：height = [1,2,1]
输出：2
```

提示：
- n = height.length
- 2 <= n <= 3 * 10^4
- 0 <= height[i] <= 3 * 10^4

## 题解
#### 题解1 双指针
算法流程： 设置双指针 i,j 分别位于容器壁两端，根据规则移动指针（后续说明），并且更新面积最大值 res，直到 i == j 时返回 res
#### 1. 代码实现
```js
var maxArea = function(height) {
    let max = 0;
    for (let i = 0, j = height.length - 1; i < j;) {
        const minHeight = height[i] < height[j] ? height[i++] : height[j--];
        const area = (j - i + 1) * minHeight;
        max = Math.max(max, area);
    }
    return max;
};
```

#### 2. 复杂度分析
- 时间复杂度 O(N)，双指针总计最多遍历整个数组一次。
- 空间复杂度：O(1)，只需要额外的常数级别的空间。

## 高赞题解
[双指针法正确性证明](https://leetcode-cn.com/problems/container-with-most-water/solution/shuang-zhi-zhen-fa-zheng-que-xing-zheng-ming-by-r3/)  
[官方题解](https://leetcode-cn.com/problems/container-with-most-water/solution/sheng-zui-duo-shui-de-rong-qi-by-leetcode-solution/)  