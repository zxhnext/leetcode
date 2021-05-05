## 题目描述

原题地址：[路径总和](https://leetcode-cn.com/problems/path-sum/)

难度： 简单

给你二叉树的根节点 root 和一个表示目标和的整数 targetSum ，判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 targetSum 。

叶子节点 是指没有子节点的节点。

示例 1：
![](./img/pathsum1.jpeg)
```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
```
示例 2：
![](./img/pathsum1.jpeg)
```
输入：root = [1,2,3], targetSum = 5
输出：false
```
示例 3：
```
输入：root = [1,2], targetSum = 0
输出：false
```

提示：
- 树中节点的数目在范围 [0, 5000] 内
- -1000 <= Node.val <= 1000
- -1000 <= targetSum <= 1000

## 题解
#### 1. 解题思路
1. 在深度优先遍历的过程中，记录当前路径的节点值的和
2. 在叶子节点处，判断当前路径的节点值的和是否等于目标值

解题步骤：
1. 深度优先遍历二叉树，在叶子节点处，判断当前路径的节点值的和是否等于目标值,是就返回true
2. 遍历结束，如果没有匹配到，就返回false

#### 2. 代码实现
```js
var hasPathSum = function(root, sum) {
    if(!root) return false;
    let res = false;
    const dfs = (n, s) => {
        if (!n.left && !n.right && s === sum) {
            res = true;
        }
        if (n.left) dfs(n.left, s + n.left.val);
        if (n.right) dfs(n.right, s + n.right.val);
    }
    dfs(root, root.val);
    return res;
};
```

#### 3. 复杂度分析
时间复杂度O(n), 空间复杂度O(n)使用了函数调用堆栈