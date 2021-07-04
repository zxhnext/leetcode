## 题目描述

原题地址：[删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

**难度：中等**

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

进阶：你能尝试使用一趟扫描实现吗？

 

示例 1：
![](../img/remove_ex1.jpeg)
```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```
示例 2：
```
输入：head = [1], n = 1
输出：[]
```
示例 3：
```
输入：head = [1,2], n = 1
输出：[1]
```

提示：
- 链表中结点的数目为 sz
- 1 <= sz <= 30
- 0 <= Node.val <= 100
- 1 <= n <= sz

## 题解
#### 1. 解题思路

使用快慢指针找到要被删除的节点的上一个节点(倒数n+1个节点)，参考题解[链表中倒数第n个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/solution/lian-biao-zhong-dao-shu-di-kge-jie-dian-qccui/)，然后将该节点删除。

这里我们要注意：
1. 为了防止只有一个节点，我们可以在头部再加一个0节点，这样获取`slow.next.next`不会报错。
2. 获取第n个节点时终止条件为`fast = null`，由于我们要找的是倒数第n+1个节点，所以我们要少走一步，即走到`fast.next = null`即可。

![image.png](https://pic.leetcode-cn.com/1624548759-CpLqjg-image.png)


#### 2. 代码实现
```js
function removeNthFromEnd(head) {
    let slow, fast;
    const newHead = new ListNode(0, head);
    slow = fast = newHead;
    while (n--) {
        fast = fast.next;
    }
    while (fast.next != null) {
        fast = fast.next;
        slow = slow.next;
    }
    slow.next = slow.next.next;
    return newHead.next;
};
```

**ts版**
```ts
function removeNthFromEnd(head: ListNode | null, n: number): ListNode | null {
    let slow, fast;
    const newHead = new ListNode(0, head);
    slow = fast = newHead;
    while (n--) {
        fast = fast.next;
    }
    while (fast.next != null) {
        fast = fast.next;
        slow = slow.next;
    }
    slow.next = slow.next.next;
    return newHead.next;
};
```

#### 3. 复杂度分析
时间复杂度O(n),空间复杂度O(1)

