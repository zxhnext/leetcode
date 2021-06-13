
## 题目描述

原题地址：[合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

难度：困难

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。


示例 1：

输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
示例 2：

输入：lists = []
输出：[]
示例 3：

输入：lists = [[]]
输出：[]
 

提示：

k == lists.length
0 <= k <= 10^4
0 <= lists[i].length <= 500
-10^4 <= lists[i][j] <= 10^4
lists[i] 按 升序 排列
lists[i].length 的总和不超过 10^4


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
        if (this.heap[parentIndex] && this.heap[parentIndex].val > this.heap[index].val) { // 父节点大于当前节点
            this.swap(parentIndex, index);
            this.shiftUp(parentIndex);
        }
    }
    // 下移操作
    shiftDown(index) {
        const leftIndex = this.getLeftIndex(index);
        const rightIndex = this.getRightIndex(index);
        if (this.heap[leftIndex] && this.heap[leftIndex].val < this.heap[index].val) {
            this.swap(leftIndex, index);
            this.shiftDown(leftIndex);
        }
        if (this.heap[rightIndex] && this.heap[rightIndex].val < this.heap[index].val) {
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
        if(this.size() === 1) return this.heap.shift();
        const top = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.shiftDown(0);
        return top;
    }
    peek() {
        return this.heap[0];
    }
    size() {
        return this.heap.length;
    }
}

var mergeKLists = function(lists) {
    const res = new ListNode(0);
    let p = res;
    const h = new MinHeap();
    lists.forEach(l => {
        if(l) h.insert(l);
    })
    while(h.size()) {
        const n = h.pop();
        p.next = n;
        p = p.next;
        if(n.next) h.insert(n.next);
    }
    return res.next;
};
```