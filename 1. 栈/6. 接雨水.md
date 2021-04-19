## 原题地址
[接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

难度：困难
## 题解
### 题解1 单调栈
#### 1. 问题分析

#### 2. 解题思路

```js
var trap = function(height) {
    let ans = 0;
    const stack = [];
    const len = height.length;
    for (let i = 0; i < len; i++) {
        while (stack.length && height[i] > height[stack[stack.length - 1]]) {
            const top = stack.pop();
            if (!stack.length) {
                break;
            }
            const left = stack[stack.length - 1];
            const currWidth = i - left - 1;
            const currHeight = Math.min(height[left], height[i]) - height[top];
            ans += currWidth * currHeight;
        }
        stack.push(i);
    }
    return ans;
};
```