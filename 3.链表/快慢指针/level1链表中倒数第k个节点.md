## 题目描述

原题地址：[链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

**难度：简单**

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。


示例：
```
给定一个链表: 1->2->3->4->5, 和 k = 2.
返回链表 4->5.
```

## 题解
#### 1. 解题思路
我们使用快慢指针。设置两个指针`slow`、`fast`,设链表节点数为`n`。求倒数第`k`个节点，我们先让`fast`走`k`个节点(一次一步)。然后`fast`和`slow`再同时走(一次一步)，`fast`走到终点走过的距离为`n-k`,那么`slow`也走了`n-k`,所以此时`slow`距离尾部距离为`n - (n - k) = k`。

![image.png](https://pic.leetcode-cn.com/1624546460-LLGLvG-image.png)

#### 2. 代码实现
```js
var getKthFromEnd = function(head, k) {
    let fast, slow;
    fast = slow = head;
    while (k--) {
        fast = fast.next;
    }

    while (fast !== null) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
};
```

**ts版**
```ts
function getKthFromEnd(head: ListNode | null, k: number): ListNode | null {
    let fast, slow;
    fast = slow = head;
    while (k--) {
        fast = fast.next;
    }
    while (fast != null) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
};
```

#### 3. 复杂度分析
时间复杂度O(n),空间复杂度O(1)