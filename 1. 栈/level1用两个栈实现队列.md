## 原题地址
[用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

难度：简单

## 题解
#### 1. 解题思路
维护两个栈，第一个栈支持插入操作，第二个栈支持删除操作

```js
var CQueue = function() {
    this.stackA = [];
    this.stackB = [];
};

CQueue.prototype.appendTail = function(value) {
    this.stackA.push(value);
};

CQueue.prototype.deleteHead = function() {
    if(this.stackB.length){
        return this.stackB.pop();
    }else{
        while(this.stackA.length){
            this.stackB.push(this.stackA.pop());
        }
        if(!this.stackB.length){
            return -1;
        }else{
            return this.stackB.pop();
        }
    }
};
```

#### 2. 复杂度分析
时间复杂度：对于插入和删除操作，时间复杂度均为 O(1)。插入不多说，对于删除操作，虽然看起来是 O(n) 的时间复杂度，但是仔细考虑下每个元素只会「至多被插入和弹出 stack2 一次」，因此均摊下来每个元素被删除的时间复杂度仍为 O(1)。

空间复杂度：O(n)。需要使用两个栈存储已有的元素。



## 高赞题解
[官方题解](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/solution/mian-shi-ti-09-yong-liang-ge-zhan-shi-xian-dui-l-3/)  