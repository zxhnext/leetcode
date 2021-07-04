## 题目描述

原题地址：[回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

**难度：简单**


请判断一个链表是否为回文链表。

示例 1:
```
输入: 1->2
输出: false
```
示例 2:
```
输入: 1->2->2->1
输出: true
```
进阶：
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

## 题解
### 1. 题解一 递归(后序遍历)
#### 1. 解题思路
我们倒序遍历链表，如果倒序的结果和正序遍历结果相同，那么链表就是回文的。

我们知道二叉树有前、中、后序遍历，其中后序遍历是从子节点访问到根结点。而链表其实也是特殊的树(只有左子树，没有右子树)，那么我们用二叉树的后序遍历方法来遍历链表，就可以实现倒序输出。

#### 2. 代码实现
```js
let left;
var isPalindrome = function(head) {
    left = head;
    return postOrder(head);
};

function postOrder (right) {
    // 到达尾部，先定初始值为true
    if (right == null) return true; 
    let res = postOrder(right.next);
    // 后序遍历
    res = res && (right.val === left.val);
    left = left.next;
    return res;
}
```

ts版代码
```ts
function isPalindrome(head: ListNode | null): boolean {
    let preNode = head;
    return postOrder(head);
    function postOrder (head) {
        if (!head) {
            return true;
        }
        let res = postOrder(head.next);
        res = res && head.val === preNode.val;
        preNode = preNode.next;
        return res;
    }
};
```

#### 3. 复杂度分析
时间复杂度O(n), 空间复杂度O(n)，递归调用栈为n个。

### 题解2 左右指针
#### 1. 解题思路
回文链表是关于中间对称的，所以我们可以只反转后半部分链表，如果后半部分链表与前半部分相同，则是回文链表。
1. 我们先用快慢指针获取中间元素,这块参考题解[链表的中间节点(快慢指针)](https://leetcode-cn.com/problems/middle-of-the-linked-list/solution/lian-biao-de-zhong-jian-jie-dian-by-zxhn-1w0w/)。这里注意，如果节点数为偶数，则有两个中点，此时取得第二个中点。

2. 反转后一半链表，反转链表的代码可参考[反转链表(递归法+双指针迭代法)](https://leetcode-cn.com/problems/reverse-linked-list/solution/fan-zhuan-lian-biao-di-gui-fa-die-dai-fa-41ee/)

3. 循环比较前一半链表和后一半反转后的链表是否相同。以下分别为奇数与偶数时的情况。

![image.png](https://pic.leetcode-cn.com/1624548682-eVsMGc-image.png)

#### 2. 代码实现
```js
var isPalindrome = function(head) {
    // 获取中间节点
    let slow, fast;
    slow = fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    let left = head;
    // 获取反转后的链表
    let right = reverse(slow);
    // 比较是否相同
    while (right != null) {
        if (left.val != right.val) {
            return false;
        }
        left = left.next;
        right = right.next;
    }
    return true;
};

function reverse (head) {
    let pre = null, cur = head;
    while (cur != null) {
        const next = cur.next;
        cur.next = pre;
        pre = cur;
        cur = next;
    }
    return pre;
}
```

**ts版**
```ts
function isPalindrome(head: ListNode | null): boolean {
    let fast, slow;
    fast = slow = head;
    while (fast != null && fast.next !=null) {
        fast = fast.next.next;
        slow = slow.next;
    }
    let left = head;
    let right = reverse(slow);
    while (right != null) {
        if (left.val != right.val) {
            return false;
        }
        left = left.next;
        right = right.next;
    }
    return true;

    function reverse(head) {
        let pre = null, cur = head;
        while (cur != null) {
            const next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next; 
        }
        return pre;
    }
};
```

#### 3. 复杂度分析
时间复杂度：O(n)。
空间复杂度：O(1)，只用了slow,fast两个指针。
