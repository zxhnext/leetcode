## 题目描述

原题地址：[二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)  

难度：中等

给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

示例：
二叉树：[3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
返回其层序遍历结果：
```
[
  [3],
  [9,20],
  [15,7]
]
```

## 题解
#### 1. 解题思路
1. 层序遍历顺序就是广度优先遍历
2. 不过在遍历的时候需要记录当前节点所处的层级，方便将其添加到不同的数组中

解题步骤：
1. 广度优先遍历二叉树
2. 遍历过程中，记录每个节点的层级，并将其添加到不同的数组中

#### 2. 代码实现
```js
var levelOrder = function(root) {
    if(!root) return []
    const q = [[root, 0]]
    const res = []
    while(q.length) {
        const [n, level] = q.shift()
        if(!res[level]) {
            res.push([n.val])
        } else {
            res[level].push(n.val)
        }
        if(n.left) q.push([n.left, level + 1])
        if(n.right) q.push([n.right, level + 1])
    }
    return res;
};
```

#### 3. 复杂度分析
时间复杂度O(n),空间复杂度O(n)

## 高赞题解
[BFS 的使用场景总结：层序遍历、最短路径问题](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/bfs-de-shi-yong-chang-jing-zong-jie-ceng-xu-bian-l/)  