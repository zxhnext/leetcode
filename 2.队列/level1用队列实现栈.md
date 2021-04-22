## 题目描述

原题地址：[用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

难度： 简单

请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通队列的全部四种操作（push、top、pop 和 empty）。

实现 MyStack 类：
> void push(int x) 将元素 x 压入栈顶。  
> int pop() 移除并返回栈顶元素。  
> int top() 返回栈顶元素。  
> boolean empty() 如果栈是空的，返回 true ；否则，返回false。  

注意：
> 你只能使用队列的基本操作 —— 也就是 push to back、peek/pop from front、size 和 is empty 这些操作。  
> 你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。  
 

示例：
```
输入：
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 2, 2, false]

解释：
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // 返回 2
myStack.pop(); // 返回 2
myStack.empty(); // 返回 False
```

提示：
>- 1 <= x <= 9
>- 最多调用100 次 push、pop、top 和 empty
>- 每次调用 pop 和 top 都保证栈不为空

## 题解
### 题解1
#### 1. 问题分析
栈先进后出，用两个队列实现，push 方法入队1，出队时将队1元素依次出队，放入队2中，将队一中最后一个元素推出。然后再将队2元素依次放回到队1中。

#### 2. 代码实现

```js
var MyStack = function() {
    this.queue = [];
    this._queue = [];
};

MyStack.prototype.push = function(x) {
    this.queue.push(x);
};

MyStack.prototype.pop = function() {
    while(this.queue.length > 1){
        this._queue.push(this.queue.shift());
    }
    let ans = this.queue.shift();
    while(this._queue.length){
        this.queue.push(this._queue.shift());
    }
    return ans;
};

MyStack.prototype.top = function() {
    return this.queue.slice(-1)[0];
};

MyStack.prototype.empty = function() {
    return !this.queue.length;
};
```

#### 3. 复杂度分析
除pop操作外，都为o(1),pop为o(n)

### 题解2

#### 1.解题思路
两个队列实现，push操作时给队列2做push操作，然后将队列1的元素追加到队列2，最后将队列1与队列2调换。
#### 2.代码实现
```js
var MyStack = function() {
    this.queue = [];
    this._queue = [];
};

MyStack.prototype.push = function(x) {
    this._queue.push(x);
    while (this.queue.length) {
        this._queue.push(this.queue.shift())
    }
    let swap = this.queue;
    this.queue = this._queue;
    this._queue = swap;
};

MyStack.prototype.pop = function() {
    return this.queue.shift();
};

MyStack.prototype.top = function() {
    return this.queue.slice(-1)[0];
};

MyStack.prototype.empty = function() {
    return !this.queue.length;
};
```
#### 3. 复杂度分析
时间复杂度：入栈操作 O(n)，其余操作都是 O(1)。
入栈操作需要将 ${queue_1}$中的 n 个元素出队，并入队 n+1 个元素到 ${queue_2}$，共有 2n+1 次操作，每次出队和入队操作的时间复杂度都是 O(1)，因此入栈操作的时间复杂度是 O(n)。  
出栈操作对应将 ${queue_1}$的前端元素出队，时间复杂度是 O(1)。  
获得栈顶元素操作对应获得 ${queue_1}$的前端元素，时间复杂度是 O(1)。  
判断栈是否为空操作只需要判断 \${queue_1}$是否为空，时间复杂度是 O(1)。  

空间复杂度：O(n)，其中 n 是栈内的元素。需要使用两个队列存储栈内的元素

### 题解3 一个队列实现
#### 1. 解题思路
push操作时记录下当前队列长度n，然后入队列，将队列最新入队的前面的所有元素依次出队，再入队。
#### 2. 代码实现
```js
var MyStack = function() {
    this.queue = [];
};

MyStack.prototype.push = function(x) {
    const len = this.queue.length;
    this.queue.push(x);
    for (let i = 0; i < len; i++) {
        this.queue.push(this.queue.shift());
    }
};

MyStack.prototype.pop = function() {
    return this.queue.shift();
};

MyStack.prototype.top = function() {
    return this.queue.slice(-1)[0];
};

MyStack.prototype.empty = function() {
    return !this.queue.length;
};
```


#### 3. 复杂度分析
时间复杂度：入栈操作 O(n)，其余操作都是 O(1)。
入栈操作需要将队列中的 n 个元素出队，并入队 n+1 个元素到队列，共有 2n+1 次操作，每次出队和入队操作的时间复杂度都是 O(1)，因此入栈操作的时间复杂度是 O(n)。  
出栈操作对应将队列的前端元素出队，时间复杂度是 O(1)。  
获得栈顶元素操作对应获得队列的前端元素，时间复杂度是 O(1)。  
判断栈是否为空操作只需要判断队列是否为空，时间复杂度是 O(1)。  

空间复杂度：O(n)，其中 n 是栈内的元素。需要使用一个队列存储栈内的元素。

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/implement-stack-using-queues/solution/yong-dui-lie-shi-xian-zhan-by-leetcode-solution/)  