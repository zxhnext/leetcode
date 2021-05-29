树知识详解： https://juejin.cn/post/6890086646973202439/

## 1. 什么是树
- 一种分层数据的抽象模型
- 前端工作中常见的树包括：DOM树、级联选择、树形控间

如下图级联选择器
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/80c4a32e4f6b4ce3a27e66f182b0f328~tplv-k3u1fbpfcp-watermark.image)

**链表是特殊化的树，树是特殊化的图(数据结构的存储方式只有两种:数组(顺序存储)和链表(链式存储)),摘自labuladong的算法小抄**。

我们用JS模拟一棵树
```js
const tree = {
    val: 'a',
    children: [
        {
            val: 'b',
            children: [
                {
                    val: 'd',
                    children: [],
                },
                {
                    val: 'e',
                    children: [],
                }
            ],
        },
        {
            val: 'c',
            children: [
                {
                    val: 'f',
                    children: [],
                },
                {
                    val: 'g',
                    children: [],
                }
            ],
        }
    ],
};
```

### 1.1 树的相关术语
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c3a7a7a05e7543838173d99ef3eab2b9~tplv-k3u1fbpfcp-watermark.image)

位于树顶部的节点叫作**根节点**。它没有父节点。树中的每个元素都叫作节点，节点分为**内部节点**和**外部节点**。至少有一个子节点的节点称为**内部节点**。没有子元素的节点称为**外部节点**或**叶节点**。

一个节点可以有祖先和后代。一个节点(除了根节点)的祖先包括父节点、祖父节点、曾祖父节点等。一个节点的后代包括子节点、孙子节点、曾孙节点等。例如，节点 5 的祖先有节点 7 和节点 11，后代有节点 3 和节点 6。

有关树的另一个术语是**子树**。子树由节点和它的后代构成。例如，节点 13、12 和 14 构成了 上图中树的一棵子树。

节点的一个属性是深度，节点的深度取决于它的祖先节点的数量。比如，节点 3 有 3 个祖先 节点(5、7 和 11)，它的深度为 3。

树的高度取决于所有节点深度的最大值。一棵树也可以被分解成层级。根节点在第 0 层，它 的子节点在第 1 层，以此类推。上图中的树的高度为 3(最大高度已在图中表示——第 3 层)


## 2. 树的常用操作
### 1. 深度优先搜索
- 尽可能深的搜索树的分支
步骤：
- 访问根节点
- 对根节点的 children 挨个进行深度优先遍历

如下图顺序
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b31d59f9661548be9a514f3f5e9ee6b7~tplv-k3u1fbpfcp-watermark.image)

代码实现：
```js
const dfs = root => {
  console.log(root.val)
  root.children.forEach(dfs)
}
```

### 2. 广度优先搜索
- 先访问离根节点最近的节点

步骤：
- 新建一个队列，把根节点入队
- 把队头出队并访问
- 把队头的 children 挨个入队
- 重复第二、三步，知道队列为空

如下图顺序：
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a53bbcb766904cd68501f77a41b2c38a~tplv-k3u1fbpfcp-watermark.image)

代码实现：
```js
const bfs = (root) => {
    const q = [root];
    while (q.length > 0) {
        const n = q.shift();
        console.log(n.val);
        n.children.forEach(child => {
            q.push(child);
        });
    }
};
```

## 3. 二叉树和二叉搜索树
### 3.1 基本概念
二叉树是树中每个节点最多只能有两个子节点，一个是左侧子节点，另一个是右侧子节点。这个定义有助于我们写出更高效地在树中插入、查找和删除节点的算法。

现实中的二叉树：
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d58ac93f4ae341db8135b5a1b4a6ce71~tplv-k3u1fbpfcp-watermark.image)

模拟一个二叉树：
```js
const bt = {
    val: 1,
    left: {
        val: 2,
        left: {
            val: 4,
            left: null,
            right: null,
        },
        right: {
            val: 5,
            left: null,
            right: null,
        },
    },
    right: {
        val: 3,
        left: {
            val: 6,
            left: null,
            right: null,
        },
        right: {
            val: 7,
            left: null,
            right: null,
        },
    },
};
```

### 3.2 二叉树的遍历
#### 3.2.1 先序遍历(根左右)
- 访问根节点
- 对根节点的左子树进行先序遍历
- 对根节点的右子树进行先序遍历

先序遍历是以优先于后代节点的顺序访问每个节点的。先序遍历的一种应用是打印一个结构化的文档

先序遍历访问路径：  
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/804a6181db0e40df9b845a121b6bec6f~tplv-k3u1fbpfcp-watermark.image)  
11 7 5 3 6 9 8 10 15 13 12 14 20 18 25

```js
const preorder = (root) => {
    if (!root) { return; }
    console.log(root.val);
    preorder(root.left);
    preorder(root.right);
};

preorder(bt)
```

**非递归版**
```js
const preorder = (root) => {
    if (!root) { return; }
    const stack = [root];
    while (stack.length) {
        const n = stack.pop();
        console.log(n.val);
        if (n.right) stack.push(n.right);
        if (n.left) stack.push(n.left);
    }
};
```

#### 3.2.2 中序遍历(左根右)
- 对根节点的左子树进行中序遍历
- 访问根节点
- 对根节点的右子树进行中序遍历

中序遍历访问路径：  
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/85d18d174abd4c44b180f319a2345279~tplv-k3u1fbpfcp-watermark.image)  
3 5 6 7 8 9 10 11 12 13 14 15 18 20 25


```js
const inorder = (root) => {
  if (!root) { return; }
  inorder(root.left);
  console.log(root.val);
  inorder(root.right);
}
```

**非递归版**
```js
const inorder = (root) => {
    if (!root) { return; }
    const stack = [];
    let p = root;
    while (stack.length || p) {
        while (p) {
            stack.push(p);
            p = p.left;
        }
        const n = stack.pop();
        console.log(n.val);
        p = n.right;
    }
};
```


#### 3.2.3 后序遍历(左右根)
- 对根节点的左子树进行后序遍历
- 对根节点的右子树进行后序遍历
- 访问根节点

后序遍历则是先访问节点的后代节点，再访问节点本身。后序遍历的一种应用是计算一个目录及其子目录中所有文件所占空间的大小

后序遍历访问路径：  
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4a50e66ae2ec466b8f20eb2e10dbbd50~tplv-k3u1fbpfcp-watermark.image)  
3 6 5 8 10 9 7 12 14 13 18 25 20 15 11

```js
const postorder = (root) => {
  if (!root) { return; }
  postorder(root.left);
  postorder(root.right);
  console.log(root.val);
}
```

**非递归版**
```js
const postorder = (root) => {
    if (!root) { return; }
    const outputStack = [];
    const stack = [root];
    while (stack.length) {
        const n = stack.pop();
        outputStack.push(n);
        if (n.left) stack.push(n.left);
        if (n.right) stack.push(n.right);
    }
    while(outputStack.length){
        const n = outputStack.pop();
        console.log(n.val);
    }
};
```

### 3.3 二叉搜索树
#### 3.3.1 基本概念
**二叉搜索树**(BST)是二叉树的一种，但是只允许你在左侧节点存储(比父节点)小的值， 在右侧节点存储(比父节点)大的值。

二叉搜索树数据结构的组织方式如下图：
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/07b784bac8af42368d4b8921832c3fd7~tplv-k3u1fbpfcp-watermark.image)

如下是一棵二叉搜索树  
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eccab1c8a270468cb0ffa40aa9509a8c~tplv-k3u1fbpfcp-watermark.image)

二叉搜索树的中序遍历是一种以上行顺序访问 BST 所有节点的遍历方式，也就是以从最小到最大的顺序访问所有节点。中序遍历的一种应用就是对树进行排序操作。

**注意：当二叉搜索树只有左子树或右子树时就成了链表**

#### 3.3.2 二叉搜索树代码实现
二叉树常用api:
> insert(key): 向树中插入一个新的键。  
> search(key):在树中查找一个键。如果节点存在，则返回 true;如果不存在，则返回false。  
> inOrderTraverse():通过中序遍历方式遍历所有节点。  
> preOrderTraverse():通过先序遍历方式遍历所有节点。  
> postOrderTraverse():通过后序遍历方式遍历所有节点。  
> min():返回树中最小的值/键。    
> max():返回树中最大的值/键。  
> remove(key):从树中移除某个键  

公共方法
```js
class Node {
  constructor(key) {
    this.key = key;
    this.left = undefined;
    this.right = undefined;
  }
  toString() {
    return `${this.key}`;
  }
}

const Compare = {
  LESS_THAN: -1,
  BIGGER_THAN: 1,
  EQUALS: 0
};
// 由于键可能是复杂的对象而不是数，我们使用传入二叉搜索树构造函数的 compareFn 函数来比较值
function defaultCompare(a, b) {
  if (a === b) {
    return Compare.EQUALS;
  }
  return a < b ? Compare.LESS_THAN : Compare.BIGGER_THAN;
}
```

```js
class BinarySearchTree {
  constructor(compareFn = defaultCompare) {
    this.compareFn = compareFn; // 用来比较节点值
    this.root = undefined; // 根节点
  }

  insert(key) {
    if (this.root == null) { // 树为空
      this.root = new Node(key);
    } else {
      this.insertNode(this.root, key);
    }
  }
  insertNode(node, key) {
    if (this.compareFn(key, node.key) === Compare.LESS_THAN) { // 小于根节点，左子树
      // 递归
      if (node.left == null) {
        node.left = new Node(key);
      } else {
        this.insertNode(node.left, key);
      }
    } else if (node.right == null) { // 否则右子树递归
      node.right = new Node(key);
    } else {
      this.insertNode(node.right, key);
    }
  }

  getRoot() {
    return this.root;
  }
  
  // 搜索特定值
  search(key) {
    return this.searchNode(this.root, key);
  }
  searchNode(node, key) {
    if (node == null) {
      return false;
    }
    if (this.compareFn(key, node.key) === Compare.LESS_THAN) { // 比当前节点小，在左子树搜索
      return this.searchNode(node.left, key);
    } else if (this.compareFn(key, node.key) === Compare.BIGGER_THAN) { // 比当前节点大，右子树搜索
      return this.searchNode(node.right, key);
    }
    return true;
  }

  // 中序遍历（排序）
  inOrderTraverse(callback) {
    this.inOrderTraverseNode(this.root, callback);
  }
  inOrderTraverseNode(node, callback) {
    if (node != null) {
      this.inOrderTraverseNode(node.left, callback);
      callback(node.key);
      this.inOrderTraverseNode(node.right, callback);
    }
  }

  // 先序遍历
  preOrderTraverse(callback) {
    this.preOrderTraverseNode(this.root, callback);
  }
  preOrderTraverseNode(node, callback) {
    if (node != null) {
      callback(node.key);
      this.preOrderTraverseNode(node.left, callback);
      this.preOrderTraverseNode(node.right, callback);
    }
  }
  
  // 后序遍历
  postOrderTraverse(callback) {
    this.postOrderTraverseNode(this.root, callback);
  }
  postOrderTraverseNode(node, callback) {
    if (node != null) {
      this.postOrderTraverseNode(node.left, callback);
      this.postOrderTraverseNode(node.right, callback);
      callback(node.key);
    }
  }
  
  // 获取最小值(左子树最下层)
  min() {
    return this.minNode(this.root);
  }
  minNode(node) {
    let current = node;
    while (current != null && current.left != null) {
      current = current.left;
    }
    return current;
  }
  
  // 获取最大值(右子树最下层)
  max() {
    return this.maxNode(this.root);
  }
  maxNode(node) {
    let current = node;
    while (current != null && current.right != null) {
      current = current.right;
    }
    return current;
  }
  
  // 移除节点
  remove(key) {
    this.root = this.removeNode(this.root, key);
  }
  removeNode(node, key) {
    if (node == null) {
      return undefined;
    }
    // 找到要移除的键
    if (this.compareFn(key, node.key) === Compare.LESS_THAN) {
      node.left = this.removeNode(node.left, key);
      return node;
    } else if (this.compareFn(key, node.key) === Compare.BIGGER_THAN) {
      node.right = this.removeNode(node.right, key);
      return node;
    }
    // 没有叶节点，直接修改值为undefined
    if (node.left == null && node.right == null) {
      node = undefined;
      return node;
    }
    // 有一个子节点，跳过该节点，直接将父节点指向它的指针指向子节点
    if (node.left == null) {
      node = node.right;
      return node;
    } else if (node.right == null) {
      node = node.left;
      return node;
    }
    // 有两个子节点
    // 1. 找到它右边子树中最小的节点
    const aux = this.minNode(node.right);
    // 2. 用它右侧子树中最小节点的键去更新这个节点的值(移除)
    node.key = aux.key;
    // 3. 右侧子树中的 最小节点移除
    node.right = this.removeNode(node.right, aux.key);
    // 4. 向它的父节点返回更新后节点的引用
    return node;
  }
}
```

移除叶节点过程：
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/906e51f3ca304f2696a8c6adc1f0f07a~tplv-k3u1fbpfcp-watermark.image)

移除有一个左侧或右侧子节点的节点过程：
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9810fd715ca44b558f6dbb5c470de60b~tplv-k3u1fbpfcp-watermark.image)

移除有两个子节点的节点过程：
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0acfab69657c4f448c6a7ae55674652d~tplv-k3u1fbpfcp-watermark.image)

树常用操作的时间复杂度分析：https://visualgo.net/zh/bst

## 4. 自平衡树
后续高级树章节再讲。

## 5. 树的常见应用
### 5.1 遍历JSON所有节点值
使用深度优先搜索
```JavaScript
const json = {
    a: { b: { c: 1 } },
    d: [1, 2],
};

const json = {
    a: { b: { c: 1 } },
    d: [1, 2],
};

const dfs = (n, path) => {
    console.log(n, path);
    Object.keys(n).forEach(k => {
        dfs(n[k], path.concat(k));
    });
};

dfs(json, []);
```

### 5.2 渲染Antd 的树组件
```JavaScript
const { Tree } = antd;
const { TreeNode } = Tree;

const json = [
  {
    title: '一',
    key: "1" ,
    children: [{ title: "三", key: "3", children: [] } ]
  },
  {
    title: '二',
    key: "2" ,
    children: [{ title: "四", key: "4", children: [] } ]
  }
];

class Demo extends React.Component {
  dfs = n => {
    return (
      <TreeNode title={n.title} key={n.key}>
        {n.children.map(this.dfs)}
       </TreeNode>
    )
  }
  render() {
    return (
      <Tree>
        {json.map(this.dfs)}
      </Tree>
    )
  }
}

ReactDom.render(<Demo />, mountNode)
```

## 6. leetcode常见考题
### 6.1 常见二叉树考题
#### [104\. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

**难度：简单**

题解：[二叉树的最大深度 DFS](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/er-cha-shu-de-zui-da-shen-du-dfs-by-zxhn-dhty/)

#### [111\. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

**难度：简单**

题解：[二叉树的最小深度 BFS](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/solution/er-cha-shu-de-zui-xiao-shen-du-bfs-by-zx-x6sr/)

#### [112\. 路径总和](https://leetcode-cn.com/problems/path-sum/)

**难度：简单**

题解：[路径总和 DFS]https://leetcode-cn.com/problems/path-sum/solution/lu-jing-zong-he-dfs-by-zxhnext-4kaj/

#### [N 叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)

**难度：简单**

题解：[N 叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/solution/n-cha-shu-de-hou-xu-bian-li-by-zxhnext-xiqz/)

[N 叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

**难度：简单**

题解：[N 叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/solution/n-cha-shu-de-qian-xu-bian-li-by-zxhnext-blbe/)

[N 叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

**难度：中等**

题解：[N 叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/solution/n-cha-shu-de-ceng-xu-bian-li-by-zxhnext-zl7t/)

#### [102\. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

**难度：中等**

题解：[二叉树的层序遍历BFS](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/er-cha-shu-de-ceng-xu-bian-li-bfs-by-zxh-51dl/)

#### [二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

**难度：中等**

题解：[二叉树的前序遍历(栈)](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/er-cha-shu-de-qian-xu-bian-li-zhan-by-zx-w2a4/)

#### [94\. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

**难度：中等**

题解：[二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/er-cha-shu-de-zhong-xu-bian-li-by-zxhnex-p35b/)

详解请参考：[leetcode刷题之路](https://github.com/zxhnext/leetcode/tree/master/5.%E6%A0%91)

持续更新ing...

