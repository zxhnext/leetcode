## 题目描述
原题地址：[最小栈](https://leetcode-cn.com/problems/min-stack/)

难度：简单

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。
>- push(x) —— 将元素 x 推入栈中。
>- pop() —— 删除栈顶的元素。
>- top() —— 获取栈顶元素。
>- getMin() —— 检索栈中的最小元素。
 

示例:
```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

提示：

`pop`、`top` 和 `getMin` 操作总是在**非空栈**上调用。


## 题解
### 题解1 辅助栈
#### 1. 问题分析
新建一个最小栈，每次push元素时，将当前栈的最小值放入最小栈中。即栈顶始终存放着该栈内元素最小值。图解如下

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
1. 建两个栈，一个正常的栈，一个最小栈存放当前栈最小值
2. push元素时，由于栈顶保存的永远是最小值，所以当入栈时只需要比较入栈值与最小栈栈顶值即可得出最小值,将最小值存入最小栈中。
3. 保持两个栈同步操作，出栈时也同步出栈

#### 3. 代码实现
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
push、pop、top、getMin时间复杂度均为O(1)

空间复杂度O(n)

### 题解2 一个栈实现

该解法本质上同两个栈思路是一样的，只是栈中存放的不再是单个元素，而是存放了一个对象，用对象模拟双栈效果

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
