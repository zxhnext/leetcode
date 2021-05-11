## 题目描述

原题地址: [接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

难度：困难

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

示例 1：  
![](./img/rainwatertrap.png)
```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。
``` 
示例 2：
```
输入：height = [4,2,0,3,2,5]
输出：9
```

提示：
>- n == height.length
>- 0 <= n <= 3 * 10^4
>- 0 <= height[i] <= 10^5

## 题解

这题和柱形图中的最大矩形类似，暴力解法这里不再赘述。
### 题解1 使用栈
#### 1. 解题思路
在遍历数组时维护一个栈。如果当前的条形块小于或等于栈顶的条形块，我们将条形块的索引入栈，意思是当前的条形块被栈中的前一个条形块界定。如果我们发现一个条形块长于栈顶，我们可以确定栈顶的条形块被当前条形块和栈的前一个条形块界定，因此我们可以弹出栈顶元素并且累加答案到 ans

1. 定义一个栈。
2. 遍历height。
3. 如果栈有值且height[i]大于栈顶元素，说明有了凹地。将栈顶元素出栈(设为height[top])。
    a. 如果此时栈为空，那么没有左区间，无法接雨水。
    b. 如果不为空，那么此时栈顶元素(height[left])必然大于或等于height[top]。
    c. 此时宽为i - left - 1，高为Math.min(height[left], height[i]) - height[top]，计算出接雨水面积累加起来
4. 继续循环，直到height[i] < 当前栈顶
5. 将height[i]加入栈中

#### 2. 代码实现
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

#### 3. 复杂度分析
时间复杂度：O(n)。单次遍历 O(n) ，每个条形块最多访问两次（由于栈的弹入和弹出），并且弹入和弹出栈都是 O(1) 的。  
空间复杂度：O(n)。 栈最多在阶梯型或平坦型条形块结构中占用 O(n) 的空间


另外还有双指针解法、动态规划解法等，后续章节再详细讨论
## 高赞题解
[官方题解](https://leetcode-cn.com/problems/trapping-rain-water/solution/jie-yu-shui-by-leetcode/)  
[详细通俗的思路分析，多解法](https://leetcode-cn.com/problems/trapping-rain-water/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-8/)  
[【接雨水】单调递减栈，简洁代码，动图模拟](https://leetcode-cn.com/problems/trapping-rain-water/solution/trapping-rain-water-by-ikaruga/)  
[dp解法](https://leetcode-cn.com/problems/trapping-rain-water/solution/dpjie-fa-by-wallcwr-11rp/)    