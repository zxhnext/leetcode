## 题目描述

原题地址：[用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

难度：简单

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（`push`、`pop`、`peek`、`empty`）：

实现 MyQueue 类：
>- void push(int x) 将元素 x 推到队列的末尾
>- int pop() 从队列的开头移除并返回元素
>- int peek() 返回队列开头的元素
>- boolean empty() 如果队列为空，返回 true ；否则，返回 false
 

说明：
>- 你只能使用标准的栈操作 —— 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
>- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
 

进阶：
>- 你能否实现每个操作均摊时间复杂度为 O(1) 的队列？换句话说，执行 n 个操作的总时间复杂度为 O(n) ，即使其中一个操作可能花费较长时间。
 

示例：
```
输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```
 

提示：
>- 1 <= x <= 9
>- 最多调用 100 次 push、pop、peek 和 empty
>- 假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）
## 题解
#### 1. 解题思路
两个栈解法，将一个栈当作输入栈，用于压入 `push` 传入的数据；另一个栈当作输出栈，用于 `pop` 和 `peek` 操作。每次 `pop` 或 `peek` 时，若输出栈为空则将输入栈的全部数据依次弹出并压入输出栈，这样输出栈从栈顶往栈底的顺序就是队列从队首往队尾的顺序

实现思路  
1. 维护两个栈。
2. push操作向输入栈中推入数据。
3. pop操作时，如果输出栈中没有数据，则将输入栈依次弹出压入输出栈中，再弹出输出栈栈顶元素。
4. peek操作同pop操作，只不过不需要弹出。

#### 2. 代码实现

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

#### 3. 复杂度分析
时间复杂度：`push` 和 `empty` 为 O(1)，`pop` 和 `peek` 为均摊 O(1)。对于每个元素，至多入栈和出栈各两次，故均摊复杂度为 O(1)。

空间复杂度：O(n)。其中 `n` 是操作总数。对于有 `n` 次 `push` 操作的情况，队列中会有 `n` 个元素，故空间复杂度为 O(n)


## 高赞题解
[官方题解](https://leetcode-cn.com/problems/implement-queue-using-stacks/solution/yong-zhan-shi-xian-dui-lie-by-leetcode/)  
[用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/solution/tu-jie-guan-fang-tui-jian-ti-jie-yong-zh-4hru/)  
[啥是「均摊复杂度」呀？我的算法击败 100%，是 O(1) 算法了吧？](https://leetcode-cn.com/problems/implement-queue-using-stacks/solution/sha-shi-jun-tan-fu-za-du-ya-wo-de-suan-f-gb6d/)  
