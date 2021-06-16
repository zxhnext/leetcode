## 题目描述

原题地址：[全排列](https://leetcode-cn.com/problems/permutations/)

**难度：中等**

给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。

示例 1：
```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```
示例 2：
```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```
示例 3：
```
输入：nums = [1]
输出：[[1]]
```

提示：
- 1 <= nums.length <= 6
- -10 <= nums[i] <= 10
- nums 中的所有整数 互不相同

## 题解

#### 1. 解题思路
1. 要求: 1、所有排列情况; 2、没有重复元素。
2. 有出路、有死路。
3. 考虑使用回溯算法，套用代码模版
```js
result = [];
function backtrack (path, list) {
    if (满足条件) {
        result.push(path);
        return
    }
    
    for () {
        // 做选择(前序遍历)
        backtrack (path, list)
        // 撤销选择(后续遍历)
    }
}
```

解题步骤：
1. 用递归模拟出所有情况。
2. 遇到包含重复元素的情况，就回溯。
3. 收集所有到达递归终点的情况，并返回。

#### 2. 代码实现
```js
var permute = function(nums) {
    const res = [];
    backtrack(nums, []);
    return res;
    function backtrack(nums, track) {
        if (track.length === nums.length) {
            res.push(track);
            return;
        }
        for (let i = 0; i < nums.length; i++) {
            if (track.includes(nums[i])) { continue; };
            track.push(nums[i]);
            const newTrack = [...track];
            backtrack(nums, newTrack);
            track.pop();

        }
    }
};
```

优化下：
```js
var permute = function(nums) {
    const res = [];
    backtrack([]);
    return res;
    function backtrack(path) {
        if(path.length === nums.length) {
            res.push(path);
            return;
        }
        for (let i = 0; i < nums.length; i++) {
            if (path.includes(nums[i])) { continue; }
            backtrack(path.concat(nums[i]));
        }
    }
};
```

#### 3. 复杂度分析
时间复杂度O(n!) n!=1*2*3*4...*(n-1)*n
空间复杂度o(n)(递归深度)

## 高赞题解
[回溯算法入门级详解 + 练习（持续更新）](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/)  

[全排列](https://leetcode-cn.com/problems/permutations/solution/quan-pai-lie-by-leetcode-solution-2/)