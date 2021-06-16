## 题目描述

原题地址：[子集](https://leetcode-cn.com/problems/subsets/)

**难度：中等**

给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

示例 1：
```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```
示例 2：
```
输入：nums = [0]
输出：[[],[0]]
```

提示：
- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10
- nums 中的所有元素 互不相同

## 题解
#### 1. 解题思路
1. 要求: 1、所有子集; 2、没有重复元素。
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
2. 保证接的数字都是后面的数字。
3. 收集所有到达递归终点的情况，并返回。

#### 2. 代码实现
```js
var subsets = function(nums) {
    const res = [];
    for(let i = 0; i <= nums.length; i++) {
        backtrack([], i, 0);
    }
    return res;
    function backtrack(track, len, start) {
        if(track.length === len) {
            res.push(track);
            return;
        }
        for(let i = start; i < nums.length; i++) {
            track.push(nums[i]);
            const newTrack = [...track];
            backtrack(newTrack, len, i + 1);
            track.pop();
        }
    }
};
```

优化
```js
var subsets = function(nums) {
    const res = [];
    for(let i = 0;i <= nums.length;i++) {
        backtrack([], i, 0);
    }
    return res;
    function backtrack (track, len, start) {
        if(track.length === len) {
            res.push(track);
            return;
        }
        for(let i = start; i < nums.length; i++) {
            backtrack(track.concat(nums[i]), len, i + 1);
        }
    }
};
```

#### 3. 复杂度分析
时间复杂度O(2 ^ n),因为每个元素都有两种可能，存在/不存在,空间复杂度O(n)