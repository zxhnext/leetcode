## 题目描述

原题地址：[每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

**难度：中等**

请根据每日 气温 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 0 来代替。

例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

提示：气温 列表长度的范围是 [1, 30000]。每个气温的值的均为华氏度，都是在 [30, 100] 范围内的整数。

## 问题分析
这道题其实是求**下一个最大元素**（Next Greater Number）问题，使用单调栈。想象一下，如果把数组里的数组比作人站队拍成一列，那么第一个能看到的人就是下一个最大元素。

```

                    |------
--------------------|      |
|       -------|    |      |
|       |      |    |      |
2       1      2    4      3
4       2      4    -1     -1
```

#### 1. 解题步骤
单调栈代码模版
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

#### 2. 代码实现
```js
var dailyTemperatures = function(T) {
    const len = T.length;
    let res = Array(len).fill(0);
    const stack = [];
    for (let i = len - 1; i >= 0; i--) {
        while (stack.length && T[i] >= T[stack[stack.length - 1]]) {
            stack.pop();
        }
        stack.length && (res[i] = stack[stack.length - 1] - i);
        stack.push(i);
    }
    return res;
};
```

**ts版**
```ts
function dailyTemperatures(temperatures: number[]): number[] {
    const len = temperatures.length;
    const res: number[] = new Array(len).fill(0);
    const stack: number[] = [];
    for (let i = len - 1; i >= 0; i--) {
        while(
            stack.length && 
            temperatures[i] >= temperatures[stack[stack.length - 1]]
        ) {
            stack.pop();
        }
        stack.length && (res[i] = stack[stack.length - 1] - i);
        stack.push(i);
    }
    return res;
};
```

#### 3. 复杂度分析
时间复杂度：这个算法的时间复杂度不是 O(n^2)，而是 O(n)。因为从整体来看：总共有 n 个元素，每个元素都被 `push` ⼊栈了⼀次，⽽最多会被 `po`p ⼀次。所以总的计算规模是和元素规模 n 成正⽐的，也就是 O(n) 的复杂度

空间复杂度：O(n)，其中 n 是温度列表的长度。需要维护一个单调栈存储温度列表中的下标。

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/daily-temperatures/solution/mei-ri-wen-du-by-leetcode-solution/)  
[LeetCode 图解 | 739.每日温度](https://leetcode-cn.com/problems/daily-temperatures/solution/leetcode-tu-jie-739mei-ri-wen-du-by-misterbooo/)  
[单调栈解决 Next Greater Number 一类问题](https://leetcode-cn.com/problems/next-greater-element-i/solution/dan-diao-zhan-jie-jue-next-greater-number-yi-lei-w/)  