## 题目描述

原题地址：[括号生成](https://leetcode-cn.com/problems/generate-parentheses/)  

难度：中等

数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

示例 1：
```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```
示例 2：
```
输入：n = 1
输出：["()"]
```

提示：
- 1 <= n <= 8

## 题解

```js
var generateParenthesis = function(n) {
    const result = [];
    const _generate = (left, right, n, s) => {
        if (left === n && right === n) {
            result.push(s);
            return;
        }

        if (left < n) _generate(left+1, right, n, s+'(');
        if (left > right) _generate(left, right+1, n, s+')');
    }
    _generate(0, 0, n, '');
    return result;
};
```