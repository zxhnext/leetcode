## 题目描述

原题地址：[跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)

**难度：中等**

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

假设你总是可以到达数组的最后一个位置。

 

示例 1:
```
输入: [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```
示例 2:
```
输入: [2,3,0,1,4]
输出: 2
```

提示:
- 1 <= nums.length <= 1000
- 0 <= nums[i] <= 10^5


## 题解
#### 1. 解题思路
贪心算法，每次都选择跳最远距离

#### 2. 代码实现
```js
var jump = function(nums) {
    const length = nums.length;
    let end = 0;
    let maxPosition = 0; 
    let steps = 0;
    for (let i = 0; i < length - 1; i++) {
        maxPosition = Math.max(maxPosition, i + nums[i]); 
        if (i == end) {
            end = maxPosition;
            steps++;
        }
    }
    return steps;
};
```

#### 3. 复杂度分析
时间复杂度：O(n)，空间复杂度：O(1)

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/jump-game-ii/solution/tiao-yue-you-xi-ii-by-leetcode-solution/)  
[【跳跃游戏 II】别想那么多，就挨着跳吧 II](https://leetcode-cn.com/problems/jump-game-ii/solution/45-by-ikaruga/)  
[详细通俗的思路分析，多解法](https://leetcode-cn.com/problems/jump-game-ii/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-10/)