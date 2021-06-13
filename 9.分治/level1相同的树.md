## 题目描述

原题地址:[相同的树](https://leetcode-cn.com/problems/same-tree/)

**难度：简单**

给你两棵二叉树的根节点 p 和 q ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。


示例 1：
![](./img/ex1.jpeg)
```
输入：p = [1,2,3], q = [1,2,3]
输出：true
```
示例 2：
![](./img/ex1.jpeg)
```
输入：p = [1,2], q = [1,null,2]
输出：false
```
示例 3：
![](./img/ex1.jpeg)
```
输入：p = [1,2,1], q = [1,1,2]
输出：false
```

提示：

两棵树上的节点数目都在范围 [0, 100] 内
-10^4 <= Node.val <= 10^4

## 题解

#### 1. 解题思路
1. 两个树相同：根节点的值相同，左子树相同，右子树相同
2. 符合`分`、`解`、`合`特性
3. 使用分而治之

解题步骤：
1. 分:获取两个树的左子树和右子树。
2. 解:递归地判断两个树的左子树是否相同，右子树是否相同。
3. 合:将上述结果合并，如果根节点的值也相同，树就相同。

#### 2. 代码实现
```js
var isSameTree = function(p, q) {
    if (p == null && q == null) {
        return true;
    } else if (p == null || q == null) {
        return false;
    } else if (p.val !== q.val) {
        return false;
    } else {
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
```

#### 3. 复杂度分析
时间复杂度O(n), 空间复杂度O(n)