## 题目描述

原题地址：[三数之和](https://leetcode-cn.com/problems/3sum/)

难度：中等

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

示例 1：
```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
```
示例 2：
```
输入：nums = []
输出：[]
```
示例 3：
```
输入：nums = [0]
输出：[]
```

提示：
- 0 <= nums.length <= 3000
- -10^5 <= nums[i] <= 10^5

## 题解
#### 1. 解题思路 排序 + 双指针
如果我们使用三重循环枚举，时间复杂度为O(N^3)。这里的关键是怎么降低重复。

我们首先将数组排序，这样可以保证(a,b,c) 这个顺序会被枚举到，而 (b,a,c)、(c,b,a) 等等这些不会，这样就减少了重复

另外，对于每一重循环而言，相邻两次枚举的元素不能相同，否则也会造成重复。如[1, 2, 2, 3, 4],所以遇到这种情况时，我们需要跳过。

遍历一次，以i + 1为左子针，length - 1为右指针，向中间合拢，遇到重复元素跳过
#### 2. 代码实现
```js
var threeSum = function(nums) {
    // 最左侧值为定值，右侧所有值进行两边推进计算
    let res = [];
    // 注意，一定要加return a -b ,否则是对字符串排序，不是数字排序
    nums.sort((a, b) => a - b);
    let size = nums.length;
    // 保证有正数负数
    if (nums[0] <= 0 && nums[size - 1] >= 0) {
        let i = 0;
        while (i < size - 2) {
            if (nums[i] > 0) break; // 最左侧大于0，无解
            let first = i + 1; // 左指针
            let last = size - 1; // 右指针
            while (first < last) {
                if (nums[i] * nums[last] > 0) break; // 三数同符号，无解
                let sum = nums[i] + nums[first] + nums[last];
                if (sum === 0) {
                    res.push([nums[i], nums[first], nums[last]]);
                }
                if (sum <= 0) {
                    // 负数过小，first右移
                    while (nums[first] === nums[++first]) {} // 重复值跳过
                } else {
                    // 正数过大，last左移
                    while (nums[last] === nums[--last]) {} // 重复值跳过
                }
            }
            // 重复值跳过
            while (nums[i] === nums[++i]) {}
        }
    }
    return res;
};
```

#### 3. 复杂度分析
- 时间复杂度：O(N^2)，其中 N 是数组 nums 的长度。
- 空间复杂度：O(logN)。我们忽略存储答案的空间，额外的排序的空间复杂度为 O(logN)。然而我们修改了输入的数组 nums，在实际情况下不一定允许，因此也可以看成使用了一个额外的数组存储了 nums 的副本并进行排序，空间复杂度为 O(N)。


## 高赞题解
[画解算法：15. 三数之和](https://leetcode-cn.com/problems/3sum/solution/hua-jie-suan-fa-15-san-shu-zhi-he-by-guanpengchn/)  
[官方题解](https://leetcode-cn.com/problems/3sum/solution/san-shu-zhi-he-by-leetcode-solution/)  
[三数之和（排序+双指针，易懂图解）](https://leetcode-cn.com/problems/3sum/solution/3sumpai-xu-shuang-zhi-zhen-yi-dong-by-jyd/)  

