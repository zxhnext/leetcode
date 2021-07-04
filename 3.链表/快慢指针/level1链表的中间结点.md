## 题目描述

原题地址：[链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

**难度：简单**

给定一个头结点为 head 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

示例 1：
```
输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
```
示例 2：
```
输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。
```

提示：
- 给定链表的结点数介于 1 和 100 之间

## 题解
#### 1. 解题思路
使用快慢双指针，fast一次走两步，slow一次走一步，当fast走到终点时，slow刚好走到中点。

![image.png](https://pic.leetcode-cn.com/1624374530-qomolh-image.png)

上图分别画出了奇数和偶数的情况。

#### 2. 代码实现
```js
var middleNode = function(head) {
    let fast, slow;
    fast = slow = head;
    while (fast && fast.next) {
        fast = fast.next.next;
        slow = slow.next;
    }
    return slow;
};
```

**ts版**
```ts
function middleNode(head: ListNode | null): ListNode | null {
    let fast, slow;
    fast = slow = head;
    while (fast && fast.next) {
        fast = fast.next.next;
        slow = slow.next;
    }
    return slow;
};
```

#### 3. 复杂度分析
时间复杂度：O(N)，空间复杂度：O(1)