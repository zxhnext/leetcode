## 题目描述

原题地址：[跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

**难度：中等**

给定一个非负整数数组 nums ，你最初位于数组的 第一个下标 。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标。

示例 1：
```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```
示例 2：
```
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

提示：
- 1 <= nums.length <= 3 * 10^4
- 0 <= nums[i] <= 10^5

## 题解
#### 1. 解题思路
1. 遍历数组中的每一个位置，实时最远可以到达的位置。
2. 如果最大位置大于等于数组长度，则结束，返回true，否则返回false

#### 2. 代码实现
```js
var canJump = function(nums) {
    const n = nums.length;
    let rightmost = 0;
    for (let i = 0; i < n; i++) {
        if (i <= rightmost) {
            rightmost = Math.max(rightmost, i + nums[i]);
            if (rightmost >= n - 1) {
                return true;
            }
        }
    }
    return false;
};
```

#### 3. 复杂度分析 
时间复杂度：O(n),空间复杂度：O(1)

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/jump-game/solution/tiao-yue-you-xi-by-leetcode-solution/)