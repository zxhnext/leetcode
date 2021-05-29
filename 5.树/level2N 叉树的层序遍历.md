## 题目描述

原题地址：[N 叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

难度：中等

给定一个 N 叉树，返回其节点值的层序遍历。（即从左到右，逐层遍历）。

树的序列化输入是用层序遍历，每组子节点都由 null 值分隔（参见示例）。

示例 1：
![](./img/narytreeexample.png)
```
输入：root = [1,null,3,2,4,null,5,6]
输出：[[1],[3,2,4],[5,6]]
```
示例 2：
![](./img/sample_4_964.png)
```
输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：[[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]
```

提示：
- 树的高度不会超过 1000
- 树的节点总数在 [0, 10^4] 之间

#### 代码实现
```js
var levelOrder = function(root) {
    const result = [];
    if (root == null) return result;
    const queue = [];
    queue.push(root);
    while (queue.length) {
        const level = [];
        const len = queue.length;
        for (let i = 0; i < len; i++) {
            const node = queue.shift();
            level.push(node.val);
            queue.push(...node.children);
        }
        result.push(level);
    }
    return result;
};
```

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/solution/ncha-shu-de-ceng-xu-bian-li-by-leetcode/)