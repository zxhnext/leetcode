## 题目描述

原题地址：[数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

```js
class MinHeap {
    constructor() {
        this.heap = [];
    }
    // 交换节点
    swap(i1, i2) {
        const temp = this.heap[i1];
        this.heap[i1] = this.heap[i2];
        this.heap[i2] = temp;
    }
    // 获取父节点
    getParentIndex(i) {
        // return Math.floor((i - 1) / 2);
        return (i - 1) >> 1; // 2进制操作，取商
    }
    getLeftIndex(i) {
        return i * 2 + 1;
    }
    getRightIndex(i) {
        return i * 2 + 2;
    }
    // 上移
    shiftUp(index) {
        if (index == 0) { return; }
        const parentIndex = this.getParentIndex(index);
        if (this.heap[parentIndex] > this.heap[index]) { // 父节点大于当前节点
            this.swap(parentIndex, index);
            this.shiftUp(parentIndex);
        }
    }
    // 下移操作
    shiftDown(index) {
        const leftIndex = this.getLeftIndex(index);
        const rightIndex = this.getRightIndex(index);
        if (this.heap[leftIndex] < this.heap[index]) {
            this.swap(leftIndex, index);
            this.shiftDown(leftIndex);
        }
        if (this.heap[rightIndex] < this.heap[index]) {
            this.swap(rightIndex, index);
            this.shiftDown(rightIndex);
        }
    }
    // 将值插入堆的底部，即数组的尾部。
    // 然后_上移:将这个值和它的父节点进行交换，直到父节点小于等于这个插入的值
    // 大小为k的堆中插入元素的时间复杂度为O(logK)
    insert(value) {
        this.heap.push(value);
        this.shiftUp(this.heap.length - 1);
    }
    // 删除堆顶
    // 用数组尾部元素替换堆顶(直接删除堆顶会破坏堆结构)。
    // 然后下移:将新堆顶和它的子节点进行交换，直到子节点大于等于这个新堆顶。
    // 大小为k的堆中删除堆顶的时间复杂度为O(logK)。
    pop() {
        this.heap[0] = this.heap.pop();
        this.shiftDown(0);
    }
    peek() {
        return this.heap[0];
    }
    size() {
        return this.heap.length;
    }
}


var findKthLargest = function(nums, k) {
    const h = new MinHeap()
    nums.forEach(n => {
        h.insert(n);
        if(h.size() > k) {
            h.pop()
        }
    })
    return h.peek()
};
```