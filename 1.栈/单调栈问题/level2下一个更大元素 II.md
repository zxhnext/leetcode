## 题目描述

原题地址：[下一个更大元素II](https://leetcode-cn.com/problems/next-greater-element-ii/)  

**难度：中等**

给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 x 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1。

示例 1:
```
输入: [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
```
注意: 输入数组的长度不会超过 10000。

## 题解
#### 1. 解题思路
next greater element(下一个更大的元素)问题，可以用单调栈求解，先附上单调栈问题代码模版。
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
这道题数组是环形的，下一个最大元素也可能出现在元素左边，最简单的方法，我们可以通过将数组放大一倍来解决这个问题。

但是在不改变数组长度的情况下，我们还可以将循环扩大一倍，再通过 % 运算符求余数，来模拟环形效果。代码如下：

#### 2. 代码实现
```js
var nextGreaterElements = function(nums) {
    const len = nums.length;
    const stack = [];
    const res = new Array(len).fill(-1);
    for (let i = 2 * len - 1; i >= 0; i--) {
        while (stack.length && nums[i%len] >= stack[stack.length - 1]) {
            stack.pop();
        }
        stack.length && (res[i%len] = stack[stack.length - 1]);
        stack.push(nums[i%len]);
    }
    return res;
};
```

**ts版**
```ts
function nextGreaterElements(nums: number[]): number[] {
    const len = nums.length;
    const stack: number[] = [];
    const res: number[] = new Array(len).fill(-1);

    for (let i = len * 2 - 1; i >= 0; i--) {
        while (stack.length && nums[i%len] >= stack[stack.length - 1]) {
            stack.pop();
        }
        stack.length && (res[i%len] = stack[stack.length - 1]);
        stack.push(nums[i%len]);
    }

    return res;
};
```

#### 3. 复杂度分析
时间复杂度: O(n)，其中 n 是序列的长度。遍历2*n次。

空间复杂度: O(n)，其中 n 是序列的长度。空间复杂度主要取决于栈的大小，栈的大小至多为 2n−1

## 高赞题解
[单调栈解决 Next Greater Number 一类问题](https://leetcode-cn.com/problems/next-greater-element-i/solution/dan-diao-zhan-jie-jue-next-greater-number-yi-lei-w/)  