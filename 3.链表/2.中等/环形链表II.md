## 题目描述

原题地址：[环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

**难度：中等**

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

说明：不允许修改给定的链表。

进阶：

你是否可以使用 O(1) 空间解决此题？
 

示例 1：
![](../img/circularlinkedlist.png)
```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

示例 2：

![](../img/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

示例 3：
![](../img/circularlinkedlist_test3.png)
```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```
 
提示：
- 链表中节点的数目范围在范围 [0, 10^4] 内
- -10^5 <= Node.val <= 10^5
- pos 的值为 -1 或者链表中的一个有效索引

## 题解
#### 1.解题思路
1. 首先判断链表中是否存在环，我们使用快慢指针(fast, slow)，如果链表中有环，两个指针一定会重合，环形链表的部分参考这篇题解，这里不再赘述。[环形链表(快慢指针)](https://leetcode-cn.com/problems/linked-list-cycle/solution/huan-xing-lian-biao-kuai-man-zhi-zhen-by-zf1b/)

2. 寻找入环点。
![image.png](https://pic.leetcode-cn.com/1624322524-uNvyFT-image.png)


根据上图，设环外距离为`a`, 环的入口为点`p1`, 两指针相遇点为`p2`, `p1->p2`距离为`b`, `p2->p1`距离为`c`，则环的周长为`b+c`。

由以上条件得两个指针在`p2`重合时slow走过的距离为`a+b`，设此时fast在环中跑了n圈，那么fast走过的距离为`a+n(b+c)+b`。由于fast速度是slow2倍，则可以得出下列公式：
`2(a+b) = a+n(b+c)+b`,
展开得：`a = (n - 1)b + nc`
整理：`a = (n - 1)b + (n - 1)c + c`
化简：`a = (n - 1)(b + c) + c`

从上公式我们可以得出`a`的长度等于`(n - 1) * 环的周长 + c`。来想一下，我们设置2个指针`s1`、`s2`以相同速度跑，当s1跑完a的距离时，那s2跑了`(n - 1) * 环的周长 + c`，此时`s1`、`s2`一定相遇于点`p1`。

看完以上推导，我们一定会有一个疑问，为什么slow走过的距离为`a+b`，而不是`a+m(b+c)+b`(设m为slow在环中跑过的圈数)。这个问题其实就是问为什么两个指针相遇时，慢指针一定没跑完一圈。

我们来思考一下，假设环周长为`g`，fast速度是slow的两倍，也就是说当同样时间内，假设两点都从p1起跑，当slow跑完一圈回到p1时，fast一定跑了两圈(第二次到达p1点)。而现实情况是，当slow进入环中(到达p1点开始跑时)，fast指针可能在环上任何位置，我们设该位置到p1距离为`k`(k<=g),那么fast第二次再到p1时走过的距离为`g+k`(一整圈加上k的长度)，而此时slow走过的距离为`(g+k)/2`，因为`k<=g`，所以`(g+k)/2 <= g`，也就是说，fast第二次到p1的时候，slow还没到p1，fast已经超过了slow，而超过时必然有相遇，相遇时slow还没走完一圈。

#### 2. 代码实现
```js
var detectCycle = function(head) {
    let fast, slow;
    fast = slow = head;
    while (true) {
        if (!fast || !fast.next) {
            return null;
        }
        fast = fast.next.next;
        slow = slow.next;
        if (slow === fast) {
            break;
        }
    }
    slow = head;
    while (slow !== fast) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
};
```

**ts版**
```ts
function detectCycle(head: ListNode | null): ListNode | null {
    let fast, slow;
    fast = slow = head;
    while (fast && fast.next) {
        fast = fast.next.next;
        slow = slow.next;
        if (fast === slow) {
            slow = head;
            while (slow !== fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;
        };
    } 
    return null;
};
```

#### 3. 复杂度分析
时间复杂度：O(N)，判断是否有环与入环点时间复杂度都为O(n),综合时间复杂度O(n)+O(n),还是O(n)。

空间复杂度：O(1)

## 参考题解
[官方题解](https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/huan-xing-lian-biao-ii-by-leetcode-solution/)  
[代码随想录」142. 环形链表 II ：简化公式，简单易懂！](https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/142-huan-xing-lian-biao-ii-jian-hua-gong-shi-jia-2/)
