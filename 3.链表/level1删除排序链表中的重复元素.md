## 题目描述

原题地址：[删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

存在一个按升序排列的链表，给你这个链表的头节点 head ，请你删除所有重复的元素，使每个元素 只出现一次 。

返回同样按升序排列的结果链表。

示例 1：
![](./img/list1.jpeg)
```
输入：head = [1,1,2]
输出：[1,2]
```
示例 2：
![](./img/list2.jpeg)
```
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```

提示：
- 链表中节点数目在范围 [0, 300] 内
- -100 <= Node.val <= 100
- 题目数据保证链表已经按升序排列

## 题解
#### 1. 解题思路
1. 因为链表是有序的，所以重复元素一定相邻。
2. 遍历链表,如果发现当前元素和下个元素值相同,就删除下个元素值

3. 遍历链表，如果发现当前元素和下个元素值相同，就删除下个元素值
4. 遍历结束后，返回原链表的头部

#### 2. 代码实现
```js
var deleteDuplicates = function(head) {
    let p = head;
    while(p && p.next) {
        if(p.val === p.next.val) {
            p.next = p.next.next;
        } else {
            p = p.next;
        }
    }
    return head;
};
```

#### 3. 复杂度分析
时间复杂度O(n), 空间复杂度O(1)