## 题目描述

原题地址：[二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)  

难度：中等

给定一个二叉树的根节点 root ，返回它的 中序 遍历。

示例 1：
![](./img/inorder_1.jpeg)
```
输入：root = [1,null,2,3]
输出：[1,3,2]
```
示例 2：
```
输入：root = []
输出：[]
```
示例 3：
```
输入：root = [1]
输出：[1]
```
示例 4：

![](./img/inorder_4.jpeg)
```
输入：root = [1,2]
输出：[2,1]
```
示例 5：
![](./img/inorder_5.jpeg)
```
输入：root = [1,null,2]
输出：[1,2]
```

提示：

树中节点数目在范围 [0, 100] 内
-100 <= Node.val <= 100
 

进阶: 递归算法很简单，你可以通过迭代算法完成吗？

## 题解
#### 解题思路
迭代法，用栈实现

```js
var inorderTraversal = function(root) {
    // 迭代版，用栈实现
    const res = []
    const stack = []
    let p = root
    while(stack.length || p) {
        while(p) {
            stack.push(p)
            p = p.left
        }
        const n = stack.pop()
        res.push(n.val)
        p = n.right
    }
    return res
};
```

#### 复杂度分析
时间复杂度O(n), 空间复杂度O(n)

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/er-cha-shu-de-zhong-xu-bian-li-by-leetcode-solutio/)