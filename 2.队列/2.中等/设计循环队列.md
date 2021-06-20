## 题目描述

原题地址: [设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)  

难度：中等

设计你的循环队列实现。 循环队列是一种线性数据结构，其操作表现基于 FIFO（先进先出）原则并且队尾被连接在队首之后以形成一个循环。它也被称为“环形缓冲器”。

循环队列的一个好处是我们可以利用这个队列之前用过的空间。在一个普通队列里，一旦一个队列满了，我们就不能插入下一个元素，即使在队列前面仍有空间。但是使用循环队列，我们能使用这些空间去存储新的值。

你的实现应该支持如下操作：
- MyCircularQueue(k): 构造器，设置队列长度为 k 。
- Front: 从队首获取元素。如果队列为空，返回 -1 。
- Rear: 获取队尾元素。如果队列为空，返回 -1 。
-enQueue(value): 向循环队列插入一个元素。如果成功插入则返回真。
- deQueue(): 从循环队列中删除一个元素。如果成功删除则返回真。
- isEmpty(): 检查循环队列是否为空。
- isFull(): 检查循环队列是否已满。
 

示例：
```
MyCircularQueue circularQueue = new MyCircularQueue(3); // 设置长度为 3
circularQueue.enQueue(1);  // 返回 true
circularQueue.enQueue(2);  // 返回 true
circularQueue.enQueue(3);  // 返回 true
circularQueue.enQueue(4);  // 返回 false，队列已满
circularQueue.Rear();  // 返回 3
circularQueue.isFull();  // 返回 true
circularQueue.deQueue();  // 返回 true
circularQueue.enQueue(4);  // 返回 true
circularQueue.Rear();  // 返回 4
```

提示：
- 所有的值都在 0 至 1000 的范围内；
- 操作数将在 1 至 1000 的范围内；
- 请不要使用内置的队列库。

## 题解
#### 1. 解题思路
通过数组操作模拟一个环。如下图所示(借用官方的图)
![官方的图](https://pic.leetcode-cn.com/1620728620-WYnYBF-image.png)

通过上图，我们可以发现，通过以下公式我们就可以得出对位位置。

`tailIndex = (headIndex + count − 1) mod capacity`

其中 capacity 是数组长度，count 是队列长度，headIndex 和 tailIndex 分别是队首 head 和队尾 tail 索引。

#### 2. 代码实现
```js
var MyCircularQueue = function(k) {
    this.capacity = k; // 容量
    this.queue = [];
    this.headIndex = 0; // 队列头部
    this.count = 0; // 队列长度
};

MyCircularQueue.prototype.enQueue = function(value) {
    if (this.isFull()) return false;
    this.queue[(this.headIndex + this.count) % this.capacity] = value;
    this.count++;
    return true;
};

MyCircularQueue.prototype.deQueue = function() {
    if (this.isEmpty()) return false;
    this.headIndex = (this.headIndex + 1) % this.capacity;
    this.count--;
    return true;
};

MyCircularQueue.prototype.Front = function() {
    if (this.isEmpty()) return -1;
    return this.queue[this.headIndex];
};

MyCircularQueue.prototype.Rear = function() {
    if (this.isEmpty()) return -1;
    // tailIndex=(headIndex+count−1) % capacity
    const tailIndex = (this.headIndex + this.count - 1) % this.capacity;
    return this.queue[tailIndex];
};

MyCircularQueue.prototype.isEmpty = function() {
    return (this.count == 0);
};

MyCircularQueue.prototype.isFull = function() {
    return (this.count == this.capacity);
};
```

#### 3. 复杂度分析
时间复杂度：O(1)。该数据结构中，所有方法都具有恒定的时间复杂度。

空间复杂度：O(N)，其中 N 是队列的预分配容量。循环队列的整个生命周期中，都持有该预分配的空间

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/design-circular-queue/solution/she-ji-xun-huan-dui-lie-by-leetcode/)  