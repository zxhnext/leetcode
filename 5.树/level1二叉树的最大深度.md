## 题目描述

原题地址：[二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

难度：简单

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

示例：
给定二叉树 [3,9,20,null,null,15,7]，
```
    3
   / \
  9  20
    /  \
   15   7
```
返回它的最大深度 3 。

## 解题
#### 1. 解题思路
1. 求最大深度，考虑使用深度优先遍历
2. 在深度优先遍历的过程中，记录每个节点所在的层级，找出最大的层级即可

解题步骤：
1. 新建一个变量，记录最大深度
2. 深度优先遍历整棵树，并记录每个节点的层级，同时不断刷新最大深度这个变量
3. 遍历结束返回最大深度这个变量

#### 2. 代码实现
```js
var maxDepth = function(root) {
    let res = 0;
    const dfs = (n, l) => {
        if (!n) { return; }
        res = Math.max(res, l)
        dfs(n.left, l + 1)
        dfs(n.right, l + 1)
    }
    dfs(root, 1)
    return res
};
```

```js
var maxDepth = function(root) {
    if(!root) {
        return 0;
    } else {
        const left = maxDepth(root.left);
        const right = maxDepth(root.right);
        return Math.max(left, right) + 1;
    }
};
```

#### 3. 复杂度分析
时间复杂度：O(n), 空间复杂度O(n)/O(logN) 因为有递归，所以空间复杂度和执行栈有关，最好情况是O(logN),最差是O(n)

## 高赞题解
[画解算法：104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/hua-jie-suan-fa-104-er-cha-shu-de-zui-da-shen-du-b/)  
[官方题解](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/er-cha-shu-de-zui-da-shen-du-by-leetcode-solution/)  
