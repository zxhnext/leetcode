## 题目描述
原题地址：[柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

难度：困难

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

![](./img/histogram.png)

以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 [2,1,5,6,2,3]。

![](./img/histogram_area.png)

图中阴影部分为所能勾勒出的最大矩形面积，其面积为 10 个单位。


示例:
```
输入: [2,1,5,6,2,3]
输出: 10
```
## 题解
### 题解1 暴力解法 双重循环(超出时间限制)
#### 1. 问题分析
枚举宽，两层遍历，求得所有组合区间内的矩形面积
#### 2. 解题思路
1. 两层循环
2. 获取该区间内最小高度
3. 比较当前面积与上一个面积，保留最大的

#### 3. 代码实现
```js
var largestRectangleArea = function(heights) {
    const len = heights.length;
    let ans = 0;
    for (let left = 0; left < len; left++) {
        let minHeight = Infinity
        for (let right = left; right < len; right++) {
            minHeight = Math.min(minHeight, heights[right])
            ans = Math.max(ans, minHeight * (right - left + 1))
        }
    }
    return ans;
};
```
#### 4. 复杂度分析
时间复杂度O(n^2)，空间复杂度：O(n)
### 题解2 暴力法(超出时间限制)
#### 1. 问题分析
使用一重循环遍历每一根柱子，从柱子开始向两侧延伸，遇到小于该柱子的高度时，就是边界。

#### 2. 解题思路
1. 遍历每根柱子
2. 往左(右)遍历，直到遇到比该柱子小的柱子
3. 比较面积，获取每次的最大值

#### 3. 代码实现
```js
var largestRectangleArea = function(heights) {
    const len = heights.length;
    // 特判
    if (len == 0) {
        return 0;
    }

    let res = 0;
    for (let i = 0; i < len; i++) {

        // 找左边最后 1 个大于等于 heights[i] 的下标
        let left = i;
        let curHeight = heights[i];
        while (left > 0 && heights[left - 1] >= curHeight) {
            left--;
        }

        // 找右边最后 1 个大于等于 heights[i] 的索引
        let right = i;
        while (right < len - 1 && heights[right + 1] >= curHeight) {
            right++;
        }

        let width = right - left + 1;
        res = Math.max(res, width * curHeight);
    }
    return res;
}
```
#### 4. 复杂度分析
时间复杂度O(n^2)，空间复杂度：O(n)

###  题解3 单调栈
[官方题解](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/zhu-zhuang-tu-zhong-zui-da-de-ju-xing-by-leetcode-/)  
```js
var largestRectangleArea = function(heights) {
    const len = heights.length;
    if (len === 0) {
        return 0;
    }
    if (len === 1) {
        return heights[0];
    }

    let res = 0;
    const stack = [];
    for (let i = 0; i < len; i++) {
        while (stack.length && heights[stack[stack.length - 1]] > heights[i]) {
            const h = heights[stack[stack.length - 1]];
            stack.pop();
            let w = i;
            if (stack.length) {
                w = i - stack[stack.length - 1] - 1;
            }
            res = Math.max(res, h * w);
        }
        stack.push(i);
    }
    while (stack.length) {
        const h = heights[stack[stack.length - 1]];
        stack.pop();
        let w = len;
        if (stack.length) {
            w = len - stack[stack.length - 1] - 1;
        }
        res = Math.max(res, h * w);
    }
    return res;
}
```
### 题解4 单调栈+哨兵技巧
[暴力解法、栈（单调栈、哨兵技巧）](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/bao-li-jie-fa-zhan-by-liweiwei1419/)
```js
const largestRectangleArea = (heights) => {
    let maxArea = 0
    const stack = []
    heights = [0, ...heights, 0]         
    for (let i = 0; i < heights.length; i++) { 
        while (heights[i] < heights[stack[stack.length - 1]]) { // 当前bar比栈顶bar矮
        const stackTopIndex = stack.pop() // 栈顶元素出栈，并保存栈顶bar的索引
        maxArea = Math.max(               // 计算面积，并挑战最大面积
            maxArea,                        // 计算出栈的bar形成的长方形面积
            heights[stackTopIndex] * (i - stack[stack.length - 1] - 1)
        )
        }
        stack.push(i)                       // 当前bar比栈顶bar高了，入栈
    }
    return maxArea
}
```


## 高赞题解
[解释单调栈解法](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/wo-yong-qiao-miao-de-bi-yu-jiang-dan-diao-zhan-jie/)