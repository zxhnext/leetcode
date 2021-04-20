## 原题地址
[用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

难度：简单

## 题解
#### 1. 解题思路
将一个栈当作输入栈，用于压入 push 传入的数据；另一个栈当作输出栈，用于 pop 和 peek 操作。每次 pop 或 peek 时，若输出栈为空则将输入栈的全部数据依次弹出并压入输出栈，这样输出栈从栈顶往栈底的顺序就是队列从队首往队尾的顺序


```js
var MyQueue = function() {
    this.inStack = [];
    this.outStack = [];
};

MyQueue.prototype.push = function(x) {
    this.inStack.push(x);
};

MyQueue.prototype.pop = function() {
    if (!this.outStack.length) {
        this.in2out();
    }
    return this.outStack.pop();
};

MyQueue.prototype.peek = function() {
    if (!this.outStack.length) {
        this.in2out();
    }
    return this.outStack[this.outStack.length - 1];
};

MyQueue.prototype.empty = function() {
    return this.outStack.length === 0 && this.inStack.length === 0;
};

MyQueue.prototype.in2out = function() {
    while (this.inStack.length) {
        this.outStack.push(this.inStack.pop());
    }
}

```

#### 2. 复杂度分析
时间复杂度：push 和 empty 为 O(1)，pop 和 peek 为均摊 O(1)。对于每个元素，至多入栈和出栈各两次，故均摊复杂度为 O(1)。

空间复杂度：O(n)。其中 n 是操作总数。对于有 n 次 \push 操作的情况，队列中会有 n 个元素，故空间复杂度为 O(n)


## 高赞题解
[官方题解](https://leetcode-cn.com/problems/implement-queue-using-stacks/solution/yong-zhan-shi-xian-dui-lie-by-leetcode/)  
[用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/solution/tu-jie-guan-fang-tui-jian-ti-jie-yong-zh-4hru/)  