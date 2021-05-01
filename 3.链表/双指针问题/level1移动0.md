## 题目描述

原题地址：[移动0](https://leetcode-cn.com/problems/move-zeroes/)  

难度：简单

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:
```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```
说明:
1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。

## 题解
### 题解1
#### 1. 解题思路
遍历数组，遇到0时删除，然后再末尾追加0
缺点：每次执行splice相当于把i后的数都向前移一位，时间复杂度O(n),再加上原来的for循环，时间复杂度为O(n^2)

#### 2. 代码实现

```js
const moveZeroes = (nums) => {
    if (!nums.length) return [];
    for (let i = 0, len = nums.length; i < len; i++) {
        if (nums[i] === 0) {
            nums.splice(i, 1)
            nums.push(0);
            i--;
            len--;
        }
    }
};
```

#### 3. 复杂度分析
时间复杂度： O(n^2)

### 题解2 双指针
#### 1. 解题思路
1. 左指针左边均为非零数；
2. 右指针左边直到左指针处均为零。
3. 因此每次交换，都是将左指针的零与右指针的非零数交换，且非零数的相对顺序并未改变

#### 2. 代码实现
```js
const moveZeroes = (nums) => {
    const length = nums.length;
    let left = 0;
    let right = 0;
    while (right < length) {
        if (nums[right]) {
            const temp = nums[right];
            nums[right] = nums[left];
            nums[left] = temp;
            left++;
        }
        right++;
    }
};
```

#### 3. 复杂度分析
- 时间复杂度：O(n)，其中 n 为序列长度。每个位置至多被遍历两次。
- 空间复杂度：O(1)。只需要常数的空间存放若干变量

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/move-zeroes/solution/yi-dong-ling-by-leetcode-solution/)  
[动画演示 283.移动零](https://leetcode-cn.com/problems/move-zeroes/solution/dong-hua-yan-shi-283yi-dong-ling-by-wang_ni_ma/)  