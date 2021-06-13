## 题目描述

原题地址：[从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

难度：中等

根据一棵树的前序遍历与中序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出
> 前序遍历 preorder = [3,9,20,15,7]
> 中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：
```
    3
   / \
  9  20
    /  \
   15   7
```

```js
var buildTree = function(preorder, inorder) {
    if(preorder.length === 0 || inorder.length === 0) {
        return null;
    }
    //根据前序数组的第一个元素，就可以确定根节点
    const root = new TreeNode(preorder[0]);
    for(let i = 0;i < preorder.length; i++) {
        //用preorder[0]去中序数组中查找对应的元素
        if(preorder[0] == inorder[i]) {
            //将前序数组分成左右两半，再将中序数组分成左右两半
            //之后递归的处理前序数组的左边部分和中序数组的左边部分
            //递归处理前序数组右边部分和中序数组右边部分
            const pre_left = preorder.slice(1, i + 1);
            const pre_right = preorder.slice(i + 1, preorder.length);
            const in_left = inorder.slice(0, i);
            const in_right = inorder.slice(i + 1, inorder.length);
            root.left = buildTree(pre_left,in_left);
            root.right = buildTree(pre_right,in_right);
            break;
        }
    }
    return root;
};
```

[官方题解](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/cong-qian-xu-yu-zhong-xu-bian-li-xu-lie-gou-zao-9/)

[详细通俗的思路分析，多解法](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by--22/)

[动画演示 105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/dong-hua-yan-shi-105-cong-qian-xu-yu-zhong-xu-bian/)