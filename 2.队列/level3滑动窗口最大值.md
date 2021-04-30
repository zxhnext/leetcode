## 原题地址
[滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

难度：困难

给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

示例:
```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 
  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7

```

提示：

你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。


## 题解
### 题解1 单调双端队列
#### 1. 问题分析
1. 未形成窗口时，遍历给定数组中的元素，如果队列不为空且当前考察元素大于等于队尾元素，则将队尾元素移除。直到，队列为空或当前考察元素小于新的队尾元素；  
2. 当队首元素的下标小于滑动窗口左侧边界left时，表示队首元素已经不再滑动窗口内，因此将其从队首移除。  
3. 由于数组下标从0开始，因此当窗口右边界right+1大于等于窗口大小k时，意味着窗口形成。此时，队首元素就是该窗口内的最大值  

#### 2. 代码实现
```js
var maxSlidingWindow = function(nums, k) {
    if (nums.length === 0 || k === 0) return nums;
    const queue = [];
    const result = [];
    for(let right = 0; right < nums.length; right++) {
        while (queue.length && nums[right] >= nums[queue[queue.length - 1]]) {
            queue.pop();
        }

        // 存储元素下标
        queue.push(right);

        // 计算窗口左侧边界
        let left = right - k + 1;
        // 当队首元素的下标小于滑动窗口左侧边界left时
        // 表示队首元素已经不再滑动窗口内，因此将其从队首移除
        if (queue[0] < left) {
            queue.shift();
        }

        // 由于数组下标从0开始，因此当窗口右边界right+1大于等于窗口大小k时
        // 意味着窗口形成。此时，队首元素就是该窗口内的最大值
        if (right + 1 >= k) {
            result[left] = nums[queue[0]];
        }
    }
    return result;
};
```

```js
var maxSlidingWindow = function(nums, k) {
    if (nums.length === 0 || k === 0) return nums;
    const deque = [];
    const result = [];
    for (let i = 0; i < k; i++) {
        while (deque.length && deque[deque.length - 1] < nums[i]) {
            deque.pop();
        }
        deque.push(nums[i]);
    }

    result[0] = deque[0];

    for (let i = k; i < nums.length; i++) {
        if (deque[0] === nums[i - k]) {
            deque.shift();
        }
        while(deque.length && nums[i] > deque[deque.length - 1]) {
            deque.pop();
        }
        deque.push(nums[i]);
        result[i - k + 1] = deque[0];
    }

    return result;
};
```


#### 3. 复杂度分析
时间复杂度O(n)，空间复杂度：O(n)
## 高赞题解
[动画演示 单调队列 剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/solution/dong-hua-yan-shi-dan-diao-dui-lie-jian-z-unpy/)  
[剑指 Offer 59 - I. 滑动窗口的最大值（单调队列，清晰图解）](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/solution/mian-shi-ti-59-i-hua-dong-chuang-kou-de-zui-da-1-6/)  
