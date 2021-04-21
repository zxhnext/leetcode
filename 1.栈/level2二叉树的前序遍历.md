## 题目描述
原题地址：[二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

难度：中等

给你二叉树的根节点 `root` ，返回它节点值的 `前序` 遍历。

示例 1：  
![](./img/inorder_1.jpeg)
```
输入：root = [1,null,2,3]
输出：[1,2,3]
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
输出：[1,2]
```
示例 5：
![](./img/inorder_5.jpeg)
```
输入：root = [1,null,2]
输出：[1,2]
```

提示：
>- 树中节点数目在范围 [0, 100] 内
>- -100 <= Node.val <= 100

## 题解

这里只考虑栈解法，后续会专门写一个章节讲树的遍历，其他解法可参考以下高赞题解。
#### 解题思路1 栈解法
前序遍历：**根->左->右**  
定义一个栈，首先将根节点 `root` 压栈，栈顶元素出栈为当前子树的根节点，然后将该节点上的元素值加入到 `List` 集合中。然后判断该根节点的左右孩子节点是否为空，若为不为空，则分别入栈

因为前序遍历是根、左、右，所以在当前根节点左右孩子节点入栈时，应该先判断右孩子节点再判断左孩子节点，这样能够保证在紧接着的栈顶节点出栈出的是之前根节点的左孩子节点。会一直出栈左孩子节点，直到左孩子节点没有了，再找根节点的右孩子节点

#### 代码实现
```js
var preorderTraversal = function(root) {
    const res = []
    const stack = []
    if(root) stack.push(root)
    while (stack.length) {
        const n = stack.pop()
        res.push(n.val)
        if(n.right) stack.push(n.right)
        if(n.left) stack.push(n.left)
    }
    return res
};
```

#### 复杂度分析
时间复杂度O(n)  
空间复杂度O(n)  

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/er-cha-shu-de-qian-xu-bian-li-by-leetcode-solution/)  
[史上最全遍历二叉树详解](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/leetcodesuan-fa-xiu-lian-dong-hua-yan-shi-xbian-2/)  
[图解 二叉树的四种遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/tu-jie-er-cha-shu-de-si-chong-bian-li-by-z1m/)   
[二叉树的前序遍历（栈☀）](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/er-cha-shu-de-qian-xu-bian-li-zhan-by-bo-oc6q/)  
