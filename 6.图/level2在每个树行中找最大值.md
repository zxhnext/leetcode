## 题目描述

原题地址：[在每个树行中找最大值](https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/)

**难度：中等**

您需要在二叉树的每一行中找到最大的值。

示例：

输入: 

          1
         / \
        3   2
       / \   \  
      5   3   9 

输出: [1, 3, 9]

#### 1. 解题思路
使用广度优先搜索，遍历每一行，比较最大值即可。

#### 2. 代码实现
```js
var largestValues = function(root) {
    if (!root) return [];
    const result = [];
    const queue = [root];
    while (queue.length) {
        let max = -Infinity;
        for (let i = 0, len = queue.length; i < len; i++) {
            const n = queue.shift();
            max = Math.max(max, n.val)
            if (n.left) queue.push(n.left);
            if (n.right) queue.push(n.right);
        }
        result.push(max);
    }
    return result;
};
```
#### 3. 复杂度分析

## 高赞题解
[java代码BFS和DFS两种解决思路以及图文分析](https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/solution/javadai-ma-bfshe-dfsliang-chong-jie-jue-si-lu-yi-j/)  
