## 题目描述

原题地址：[反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

难度：简单

给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

示例 1：
![](./img/rev1ex1.jpeg)
```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

示例 2：
![](./img/rev1ex1.jpeg)
```
输入：head = [1,2]
输出：[2,1]
```
示例 3：
```
输入：head = []
输出：[]
```

提示：
- 链表中节点的数目范围是 [0, 5000]
- -5000 <= Node.val <= 5000
 

进阶：链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？

## 题解
### 题解1 双指针迭代法
#### 解题思路
- 反转两个节点： 将n+1的next指向n
- 反转多个节点：双指针遍历链表，重复上述操作

- 双指针一前一后遍历链表
- 反转双指针

#### 代码实现
```js
var reverseList = function(head) {
    let p1 = head;
    let p2 = null;
    while(p1) {
        const tmp = p1.next
        p1.next = p2
        p2 = p1
        p1 = tmp
    }
    return p2
};
```

#### 复杂度分析
时间复杂度O(n), 空间复杂度O(1)

### 解法2 递归法
#### 1. 解题思路
1. 使用递归函数，一直递归到链表的最后一个结点，该结点就是反转后的头结点，记作 ret.
2. 此后，每次函数在返回的过程中，让当前结点的下一个结点的 next 指针指向当前节点。
3. 同时让当前结点的 next 指针指向 NULL，从而实现从链表尾部开始的局部反转
4. 当递归函数全部出栈后，链表反转完成。

#### 2. 代码实现
```js
var reverseList = function(head) {
    let prev = null;
    let curr = head;
    while (curr) {
        const next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
};
```

#### 3. 复杂度分析
1. 时间复杂度：O(n)，假设 n 是列表的长度，那么时间复杂度为 O(n)。
2. 空间复杂度：O(n)，由于使用递归，将会使用隐式栈空间。递归深度可能会达到 n 层

## 高赞题解
[反转链表】：双指针，递归，妖魔化的双指针](https://leetcode-cn.com/problems/reverse-linked-list/solution/fan-zhuan-lian-biao-shuang-zhi-zhen-di-gui-yao-mo-/)  
[官方题解](https://leetcode-cn.com/problems/reverse-linked-list/solution/fan-zhuan-lian-biao-by-leetcode-solution-d1k2/)  