## 原题地址
[最小栈](https://leetcode-cn.com/problems/min-stack/)

难度：简单

## 题解
### 题解1 辅助栈
#### 1. 问题分析
新建一个栈，储存每个元素入栈时当前栈的最小值，即栈顶始终存放着该栈内元素最小值

```
入栈 3，minStack 入栈当前最小值3
|   |    |   |
|   |    |   |
|_3_|    |_3_|
stack  minStack

入栈 5，minStack 入栈当前最小值3
|   |    |   |
| 5 |    | 3 |
|_3_|    |_3_|
stack  minStack

入栈 2，minStack 入栈当前最小值 2 
| 2 |    | 2 |
| 5 |    | 3 |
|_3_|    |_3_|
stack  minStack

出栈 2，minStack 出栈
|   |    |   |
| 5 |    | 3 |
|_3_|    |_3_|
stack  minStack

出栈 5，minStack 出栈
|   |    |   |
|   |    |   |
|_3_|    |_3_|
stack  minStack

出栈 3，minStack 出栈  
|   |    |   |
|   |    |   |
|_ _|    |_ _|
stack  minStack
```

#### 2. 解题思路
1. 建两个栈
2. 保持两个栈操作同步，入栈时将该栈最小值保存在最小栈中，出栈时最小栈同步出栈。
3. 由于栈顶保存的永远是最小值，所以当入栈时只需要比较入栈值与最小栈栈顶值即可得出最小值
```js
var MinStack = function() {
    this.stack = []
    this.min_stack = [Infinity]
};

MinStack.prototype.push = function(val) {
    this.stack.push(val)
    this.min_stack.push(Math.min(val, this.min_stack[this.min_stack.length - 1]))
};

MinStack.prototype.pop = function() {
    this.stack.pop()
    this.min_stack.pop()
};

MinStack.prototype.top = function() {
    return this.stack[this.stack.length - 1]
};

MinStack.prototype.getMin = function() {
    return this.min_stack[this.min_stack.length - 1]
};
```

#### 4. 复杂度分析
时间复杂度，O(1)，空间复杂度O(n)

### 题解2 一个栈实现

该解法本质上同两个栈思路是一样的，只是把最小值放在了同一个栈中

```js
var MinStack = function() {
    this.stack = []
};

MinStack.prototype.push = function(val) {
    if (!this.stack.length) {
        this.stack.push({val, min: val})
    } else {
        this.stack.push({
            val, 
            min: Math.min(val, this.stack[this.stack.length - 1].min)
        })
    }
};

MinStack.prototype.pop = function() {
    this.stack.pop()
};

MinStack.prototype.top = function() {
    return this.stack[this.stack.length - 1].val
};

MinStack.prototype.getMin = function() {
    return this.stack[this.stack.length - 1].min
};
```

## 高赞题解
[双栈实现「最小栈」](https://leetcode-cn.com/problems/min-stack/solution/tu-li-zhan-shi-shuang-zhan-shi-xian-zui-fcwj5/)  
[详细通俗的思路分析，多解法](https://leetcode-cn.com/problems/min-stack/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-38/)  
[辅助栈法，清晰图解](https://leetcode-cn.com/problems/min-stack/solution/min-stack-fu-zhu-stackfa-by-jin407891080/)  