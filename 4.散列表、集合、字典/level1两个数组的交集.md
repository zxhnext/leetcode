## 题目描述

原题地址：[两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

难度： 简单

给定两个数组，编写一个函数来计算它们的交集。

示例 1：
```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```
示例 2：
```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
```

## 解题思路
### 题解1 使用集合：Set
#### 1. 解题思路
求交集且无序唯一。

1. 用集合对num1去重
2. 遍历nums1,筛选出nums2也包含的值

#### 2. 代码实现
```js
var intersection = function(nums1, nums2) {
    return [...new Set(nums1)].filter(n => nums2.includes(n))
};
```
#### 3. 复杂度分析
时间复杂度filter+includesO(m*n), 空间复杂度O(m)

### 题解2 使用字典Map
#### 1. 解题思路
1. 求nums1和nums2都有的值。
2. 用字典建立一个映射关系，记录nums1里有的值。


解题步骤：
1. 新建一个字典，遍历nums1， 填充字典
2. 遍历nums2,遇到字典里的值就选出，并从字典中删除

#### 2. 代码实现
```js
var intersection = function(nums1, nums2) {
    const map = new Map();
    nums1.forEach(n => {
        map.set(n, true)
    })
    const res = []
    nums2.forEach(n => {
        if(map.get(n)){
            res.push(n)
            map.delete(n)
        }
    })
    return res;
};
```
#### 3. 复杂度分析
时间复杂度O(m+n), 空间复杂度：O(m)

### 题解3 排序+双指针(官方解法)

以下来自官方题解

#### 1. 解题思路
1. 首先将数组排序
2. 定义一个数组保存结果result
3. 两个指针分别指向两个数组的头部，每次比较两个指针指向的两个数组中的数字，如果两个数字不相等，则将指向较小数字的指针右移一位。如果两个数字相等，且该数字不等于result的尾部元素，同时将两个指针都右移一位。当至少有一个指针超出数组范围时，遍历结束

解释下为啥与result的尾部元素比较，因为数组nums1，nums2是经过升序排序的，所以result也必然是升序的，所以新遍历的元素如果不等于result尾部元素，必然大于尾部元素，且不会与result内元素重复(因为是升序的)。


#### 2. 代码实现
```js
var intersection = function(nums1, nums2) {
    nums1.sort((a, b) => a - b);
    nums2.sort((a, b) => a - b);
    const length1 = nums1.length, length2 = nums2.length;
    let index1 = 0, index2 = 0;
    const result = [];
    while (index1 < length1 && index2 < length2) {
        const num1 = nums1[index1], num2 = nums2[index2];
        if (num1 === num2) {
            // 保证加入元素的唯一性
            if (!result.length || num1 !== result[result.length - 1]) {
                result.push(num1);
            }
            index1++;
            index2++;
        } else if (num1 < num2) {
            index1++;
        } else {
            index2++;
        }
    }
    return result;
};
```

#### 3. 复杂度分析
时间复杂度：O(mlogm+nlogn)，其中 m 和 n 分别是两个数组的长度。对两个数组排序的时间复杂度分别是 O(mlogm) 和 O(nlogn)，双指针寻找交集元素的时间复杂度是 O(m+n)，因此总时间复杂度是 O(mlogm+nlogn)。

空间复杂度：O(logm+logn)，其中 m 和 n 分别是两个数组的长度。空间复杂度主要取决于排序使用的额外空间
