## 题目描述
原题地址: [用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

难度：简单

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead`，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )

示例 1：
```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```
示例 2：
```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```
提示：

>- 1 <= values <= 10000
>- 最多会对 appendTail、deleteHead 进行 10000 次调用
## 题解
#### 1. 问题分析
栈是先进后出，队列是先进先出。维护两个栈，第一个栈支持插入操作，向队列尾部添加元素时，直接向栈顶`push`即可。 第二个栈支持删除操作。首先我们需要将第一个栈中元素倒到第二个栈中，这样栈底就变为了栈顶。然后再删除第二个栈的栈顶。

解题思路  
1. 声明两个栈
2. appendTail操作，向栈1中添加元素
3. 删除操作时，我们不需要每次都将栈1元素都出栈放入栈2中，如果栈2元素是有值的，则说明已进行过此操作，直接弹出栈2的栈顶即可。如果栈2是空的，再将栈1元素依次出栈，放入栈2中，再弹出栈2的栈顶元素。

#### 2. 代码实现

```js
var CQueue = function() {
    this.inStack = [];
    this.outStack = [];
};

CQueue.prototype.appendTail = function(value) {
    this.inStack.push(value);
};

CQueue.prototype.deleteHead = function() {
    if (!this.outStack.length) {
        while (this.inStack.length) {
            this.outStack.push(this.inStack.pop());
        }
    }
    if (this.outStack.length) {
        return this.outStack.pop();
    } else {
        return -1;
    }
};
```

#### 3. 复杂度分析
时间复杂度：对于插入和删除操作，时间复杂度均为 O(1)。插入不多说，对于删除操作，虽然看起来是 O(n) 的时间复杂度，但是仔细考虑下每个元素只会「至多被插入和弹出 stack2 一次」，因此均摊下来每个元素被删除的时间复杂度仍为 O(1)。

空间复杂度：O(n)。需要使用两个栈存储已有的元素。

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/solution/mian-shi-ti-09-yong-liang-ge-zhan-shi-xian-dui-l-3/)  
[【栈】用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/solution/tu-jie-guan-fang-tui-jian-ti-jie-yong-li-yjbf/)
