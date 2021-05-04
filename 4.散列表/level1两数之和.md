## 题目描述

原题地址：[两数之和](https://leetcode-cn.com/problems/two-sum/)

难度：简单

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。


示例 1：
```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```
示例 2：
```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```
示例 3：
```
输入：nums = [3,3], target = 6
输出：[0,1]
```

提示：
- 2 <= nums.length <= 10^3
- -109 <= nums[i] <= 10^9
- -109 <= target <= 10^9
- 只会存在一个有效答案

## 题解
#### 1. 解题思路
1. 把nums想象成相亲者
2. 把target想象成匹配条件
3. 用字典建立一个婚姻介绍所，存储相亲者的数字和下标

解题步骤：
1. 新建一个字典作为婚姻介绍所
2. nums里的值，逐个来介绍所找对象，没有合适的就先登记着，有合适的就牵手成功

#### 2. 代码实现
```js
var twoSum = function(nums, target) {
    let numObj = {}
    let twoNum
    let len = nums.length
    for(let i=0; i<len; i++) {
        twoNum = target-nums[i]
        if(numObj[twoNum] === undefined) {
            numObj[nums[i]] = i
        } else {
            return [numObj[twoNum], i]
        }
    }
};
```

#### 3. 复杂度分析
时间复杂度O(n), 空间复杂度O(n)