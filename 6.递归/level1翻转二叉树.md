## 题目描述

原题地址：[翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/description/)  

难度：简单

翻转一棵二叉树。

示例：

输入：
```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```
输出：
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

## 题解
#### 1. 解题思路
1. 先翻转左右子树，再将子树换个位置
2. 符合`分`、`解`、`合`特性
3. 使用分而治之

解题步骤
1. 分： 获取左右子树
2. 解：递归的翻转左右子树
3. 合：将翻转后的左右子树换个位置放到根节点上

#### 2. 代码实现
```js
var invertTree = function(root) {
    if(!root) { return null; }
    return {
        val: root.val,
        left: invertTree(root.right),
        right: invertTree(root.left)
    }
};
```

#### 3. 复杂度分析
时间复杂度O(n),空间复杂度O(n)(树的高度)

## 高赞题解
[动画演示 两种实现 226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/solution/dong-hua-yan-shi-liang-chong-shi-xian-226-fan-zhua/)  