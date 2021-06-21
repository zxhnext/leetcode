## 题目描述

原题地址：[下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)

**难度：简单**

给你两个 没有重复元素 的数组 nums1 和 nums2 ，其中nums1 是 nums2 的子集。

请你找出 nums1 中每个元素在 nums2 中的下一个比其大的值。

nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出 -1 。

示例 1:
```
输入: nums1 = [4,1,2], nums2 = [1,3,4,2].
输出: [-1,3,-1]
解释:
    对于 num1 中的数字 4 ，你无法在第二个数组中找到下一个更大的数字，因此输出 -1 。
    对于 num1 中的数字 1 ，第二个数组中数字1右边的下一个较大数字是 3 。
    对于 num1 中的数字 2 ，第二个数组中没有下一个更大的数字，因此输出 -1 。
```
示例 2:
```
输入: nums1 = [2,4], nums2 = [1,2,3,4].
输出: [3,-1]
解释:
    对于 num1 中的数字 2 ，第二个数组中的下一个较大数字是 3 。
    对于 num1 中的数字 4 ，第二个数组中没有下一个更大的数字，因此输出 -1 。
``` 

提示：
- 1 <= nums1.length <= nums2.length <= 1000
- 0 <= nums1[i], nums2[i] <= 10^4
- nums1和nums2中所有整数 互不相同
- nums1 中的所有整数同样出现在 nums2 中

## 题解
#### 1. 解题思路
这种next greater element(下一个更大的元素)问题都可以用单调栈求解，下面是一个单调栈问题模版。

```js
var nextGreaterElement = function (nums) {
    const len = nums.length;
    const result = new Array(len).fill(-1); // 存放答案的数组
    const stack = [];
    for (let i = len - 1; i >= 0; i--) { // 倒着往栈里放
        // 判定高矮
        while (stack.length && stack[stack.length - 1] <= nums[i]) {
            // 矮个起开，反正也被挡着了。。。
            stack.pop();
        }
        // nums[i] 身后的第一个高的
        result[i] = stack.length ? stack[stack.length - 1] : -1;
        // 进队，接受之后的⾝⾼判定吧
        stack.push(nums[i]);
    }
    return result;
}
```

解题步骤

1. 对nums2套用上面单调栈模版，但是我们这里不用result存储结果，而是使用字典存储，元素为键，它的下一个更大元素为值。
2. 遍历nums1，在字典中查找元素，如果元素，则下一个最大元素就是该键的值，没有则为-1。

#### 2. 代码实现
```js
var nextGreaterElement = function(nums1, nums2) {
    const stack = [];
    const map = new Map();
    for (let i = nums2.length - 1; i >= 0; i--) {
        while (stack.length && nums2[i] >= stack[stack.length - 1]) {
            stack.pop();
        }
        stack.length && map.set(nums2[i], stack[stack.length - 1]);
        stack.push(nums2[i]);
    }
    const res = [];
    for (let i = 0; i < nums1.length; i++) {
        res[i] = map.get(nums1[i]) || -1;
    }
    return res;
};
```

**ts版**
```ts
function nextGreaterElement(nums1: number[], nums2: number[]): number[] {
    const stack: number[] = [];
    const map = new Map();
    for (let i = nums2.length - 1; i >= 0; i--) {
        while (stack.length && nums2[i] >= stack[stack.length - 1]) {
            stack.pop()
        }
        stack.length && (map.set(nums2[i], stack[stack.length - 1]));
        stack.push(nums2[i]);
    }

    const res: number[] = [];
    for (let i = 0; i < nums1.length; i++) {
        res[i] = map.get(nums1[i]) || -1;
    }
    return res;
};
```

#### 3. 复杂度分析
时间复杂度为O(N + M)，分别遍历数组 nums1 和数组 nums2 各一次即可，对于 nums2 中的每个元素，进栈一次，出栈一次。

空间复杂度：O(N)。遍历 nums2 时，需要使用栈，以及哈希映射用来临时存储答案。

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/next-greater-element-i/solution/xia-yi-ge-geng-da-yuan-su-i-by-leetcode/)