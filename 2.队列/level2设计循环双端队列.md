## 题目描述

原题地址：[设计循环双端队列](https://leetcode-cn.com/problems/design-circular-deque/)  

难度：中等

设计实现双端队列。
你的实现需要支持以下操作：
- MyCircularDeque(k)：构造函数,双端队列的大小为k。
- insertFront()：将一个元素添加到双端队列头部。 如果操作成功返回 true。
- insertLast()：将一个元素添加到双端队列尾部。如果操作成功返回 true。
- deleteFront()：从双端队列头部删除一个元素。 如果操作成功返回 true。
- deleteLast()：从双端队列尾部删除一个元素。如果操作成功返回 true。
- getFront()：从双端队列头部获得一个元素。如果双端队列为空，返回 -1。
- getRear()：获得双端队列的最后一个元素。 如果双端队列为空，返回 -1。
- isEmpty()：检查双端队列是否为空。
- isFull()：检查双端队列是否满了。
示例：
```
MyCircularDeque circularDeque = new MycircularDeque(3); // 设置容量大小为3
circularDeque.insertLast(1);			        // 返回 true
circularDeque.insertLast(2);			        // 返回 true
circularDeque.insertFront(3);			        // 返回 true
circularDeque.insertFront(4);			        // 已经满了，返回 false
circularDeque.getRear();  				// 返回 2
circularDeque.isFull();				        // 返回 true
circularDeque.deleteLast();			        // 返回 true
circularDeque.insertFront(4);			        // 返回 true
circularDeque.getFront();				// 返回 4
```

提示：
- 所有值的范围为 [1, 1000]
- 操作次数的范围为 [1, 1000]
- 请不要使用内置的双端队列库。

## 题解
#### 1. 解题思路
尝试用对象实现。

#### 2. 代码实现
```js
var MyCircularDeque = function(k) {
    this.items = {};
    this.capacity = k;
    this.count = 0;
    this.lowestCount = 0;
};

MyCircularDeque.prototype.insertFront = function(value) {
    if (this.isFull()) return false;
    if (this.isEmpty()) {
        return this.insertLast(value);
    } else if (this.lowestCount > 0) {
        // 有元素已经被从双端队列的前端移除，lowestCount会大于等于 1。
        // 这种情况下，我们只需要将 lowestCount 属性减 1 并将新元素的值放在这个键的位置上即可
        this.lowestCount--;
        this.items[this.lowestCount] = value;
        return true;
    } else { 
        // lowestCount 为 0 的情况。
        // 在第一位添加一个 新元素，并将所有元素后移一位来空出第一个位置。
        // 在所有的元素都完成移动后，第一位将是空闲状态，这样就可以用需要添加的新元素来覆盖它了
        for (let i = this.count; i > 0; i--) {
            this.items[i] = this.items[i - 1];
        }
        this.count++;
        this.items[0] = value;
        return true;
    }
};

MyCircularDeque.prototype.insertLast = function(value) {
    if (this.isFull()) return false;
    this.items[this.count] = value;
    this.count++;
    return true;
};

MyCircularDeque.prototype.deleteFront = function() {
    if (this.isEmpty()) return false;
    delete this.items[this.lowestCount];
    this.lowestCount++;
    return true;
};

MyCircularDeque.prototype.deleteLast = function() {
    if (this.isEmpty()) return false;
    delete this.items[this.count];
    this.count--;
    return true;
};

MyCircularDeque.prototype.getFront = function() {
    if (this.isEmpty()) return -1;
    return this.items[this.lowestCount];
};

MyCircularDeque.prototype.getRear = function() {
    if (this.isEmpty()) return -1;
    return this.items[this.count - 1];
};

MyCircularDeque.prototype.isEmpty = function() {
    return this.count - this.lowestCount === 0;
};

MyCircularDeque.prototype.isFull = function() {
    return this.count - this.lowestCount >= this.capacity;
};
```