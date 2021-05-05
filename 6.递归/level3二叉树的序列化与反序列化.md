## 题目描述

原题地址：[二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)  

难度：困难

序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

提示: 输入输出格式与 LeetCode 目前使用的方式一致，详情请参阅 LeetCode 序列化二叉树的格式。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。


示例 1：
![](./img/serdeser.jpeg)
```
输入：root = [1,2,3,null,null,4,5]
输出：[1,2,3,null,null,4,5]
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
```
输入：root = [1,2]
输出：[1,2]
```

提示：
- 树中结点数在范围 [0, 10^4] 内
- -1000 <= Node.val <= 1000

## 题解

```js
var serialize = function(root) {
    if (root == null) {                  // 遍历到 null 节点
        return 'X';
    }
    const left = serialize(root.left);   // 左子树的序列化结果
    const right = serialize(root.right); // 右子树的序列化结果
    return root.val + ',' + left + ','+ right; // 按  根,左,右  拼接字符串
};

var deserialize = function(data) {
    const list = data.split(',');   // split成数组

    const buildTree = (list) => {   // 基于list构建当前子树
        const rootVal = list.shift(); // 弹出首项，获取它的“数据”
        if (rootVal == "X") {         // 是X，返回null节点
            return null;
        }
        const root = new TreeNode(rootVal); // 不是X，则创建节点
        root.left = buildTree(list);        // 递归构建左子树
        root.right = buildTree(list);       // 递归构建右子树
        return root;                        // 返回当前构建好的root
    };

    return buildTree(list); // 构建的入口
};
```

## 高赞题解
[『手画图解』剖析DFS、BFS解法 | 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/solution/shou-hui-tu-jie-gei-chu-dfshe-bfsliang-chong-jie-f/)  
[官方题解](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/solution/er-cha-shu-de-xu-lie-hua-yu-fan-xu-lie-hua-by-le-2/)  