## 题目描述

原题地址：[对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

**难度：简单**

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
```
    1
   / \
  2   2
   \   \
   3    3
```

进阶：

你可以运用递归和迭代两种方法解决这个问题吗？


#### 1. 解题思路
1. 转化为:左右子树是否镜像。
2. 分解为:树1的左子树和树2的右子树是否镜像,树1的右子树和树2的左子树是否镜像。
3. 使用分而治之

解题步骤：
1. 分:获取两个树的左子树和右子树。
2. 解:递归地判断树1的左子树和树2的右子树是否镜像，树1的右子树和树2的左子树是否镜像。
3. 合:如果上述都成立，且根节点值也相同，两个树就镜像。

#### 2. 代码实现
```js
var isSymmetric = function(root) {
    if(!root) return true;
    const isMirror = (l, r) => {
        if(!l && !r) return true; // 两个空子树为镜像
        if(
            l && r && l.val === r.val &&
            isMirror(l.left, r.right) && 
            isMirror(l.right, r.left)
        ) {
            return true;
        }
        return false;
    }
    return isMirror(root.left, root.right)
};
```

迭代版代码
```js
const check = (u, v) => {
    const q = [];
    q.push(u);
    q.push(v);

    while (q.length) {
        u = q.shift();
        v = q.shift();

        if (!u && !v) continue;
        if ((!u || !v) || (u.val !== v.val)) return false;

        q.push(u.left); 
        q.push(v.right);

        q.push(u.right); 
        q.push(v.left);
    }
    return true;
}
var isSymmetric = function(root) {
    return check(root, root);
};

```

#### 3. 复杂度分析
时间复杂度O(n),空间复杂度O(n)