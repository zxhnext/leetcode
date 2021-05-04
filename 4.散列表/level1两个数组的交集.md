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
时间复杂度filter+includesO(m*n), 空间复杂度O(m)

### 题解1 使用字典Map
#### 1. 解题思路
1. 求nums1和nums2都有的值。
2. 用字典建立一个映射关系，记录nums1里有的值。


解题步骤：
1. 新建一个字典，遍历nums1， 填充字典
2. 遍历nums2,遇到字典里的值就选出，并从字典中删除

#### 2. 代码实现
```js
var intersection = function(nums1, nums2) {
    return [...new Set(nums1)].filter(n => nums2.includes(n))
};
```

#### 3. 复杂度分析
时间复杂度O(m+n), 空间复杂度：O(m)