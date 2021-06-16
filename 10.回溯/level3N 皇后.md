## 题目描述

原题地址：[N 皇后](https://leetcode-cn.com/problems/n-queens/)

**难度：困难**

n 皇后问题 研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n ，返回所有不同的 n 皇后问题 的解决方案。

每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

示例 1：
![](./img/queens.jpeg)
```
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。
```
示例 2：
```
输入：n = 1
输出：[["Q"]]
```

提示：
- 1 <= n <= 9
- 皇后彼此不能相互攻击，也就是说：任何两个皇后都不能处于同一条横行、纵行或斜线上。

## 题解
考虑使用回溯算法，套用代码模版
```js
result = [];
function backtrack (path, list) {
    if (满足条件) {
        result.push(path);
        return
    }
    
    for () {
        // 做选择(前序遍历)
        backtrack (path, list)
        // 撤销选择(后续遍历)
    }
}
```

解题步骤：
1. 用递归模拟出所有情况。
2. 找出复合条件的选项(排查同一行/列，左上方/右上方，因为左下方/右下方还没有放皇后，所以不用考虑)。
3. 收集所有到达递归终点的情况，并返回。

#### 2. 代码实现
```js
var solveNQueens = function(n) {
    const res = [];
    const board = new Array(n);
    for (let i = 0; i < n; i++) {     // 棋盘的初始化
        board[i] = new Array(n).fill('.');
    }
    backtrack(board, 0);
    return res;

    function backtrack (board, row) {
        if (row === board.length) {
            const stringsBoard = board.slice(); // 拷贝一份board
            for (let i = 0; i < n; i++) {
                stringsBoard[i] = stringsBoard[i].join(''); // 将每一行拼成字符串
            }
            res.push(stringsBoard); // 推入res数组
            return;
        }

        for (let col = 0; col < n; col++) {
            if (isValid(board, row, col)) {
                board[row][col] = "Q";          // 作出选择，放置皇后
                backtrack(board, row + 1);                // 继续选择，往下递归
                board[row][col] = '.';   
            }
        }
    }
};

function isValid (board, row, col) {
    const n = board.length;
    // 检查列中是否有皇后互相冲突
    for (let i = 0; i < row; i++) {
        if (board[i][col] === 'Q') {
            return false;
        }
    }
    // 检查右上方是否有皇后互相冲突
    for (let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
        if (board[i][j] === 'Q') {
            return false;
        }
    }
    for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] === 'Q') {
            return false;
        }
    }
    return true;
}
```

#### 3. 复杂度分析
时间复杂度：O(N!)。 空间复杂度：O(N)。空间复杂度主要取决于递归调用层数、记录每行放置的皇后的列下标的数组以及三个集合，递归调用层数不会超过 N，数组的长度为 N，每个集合的元素个数都不会超过 N。

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/n-queens/solution/nhuang-hou-by-leetcode-solution/)  
