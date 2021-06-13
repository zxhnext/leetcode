## 题目描述

原题地址：[猜数字大小](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

难度：简单

猜数字游戏的规则如下：

- 每轮游戏，我都会从 1 到 n 随机选择一个数字。 请你猜选出的是哪个数字。
- 如果你猜错了，我会告诉你，你猜测的数字比我选出的数字是大了还是小了。

你可以通过调用一个预先定义好的接口 int guess(int num) 来获取猜测结果，返回值一共有 3 种可能的情况（-1，1 或 0）：

- -1：我选出的数字比你猜的数字小 pick < num
- 1：我选出的数字比你猜的数字大 pick > num
- 0：我选出的数字和你猜的数字一样。恭喜！你猜对了！pick == num
返回我选出的数字。

 

示例 1：
```
输入：n = 10, pick = 6
输出：6
```
示例 2：
```
输入：n = 1, pick = 1
输出：1
```
示例 3：
```
输入：n = 2, pick = 1
输出：1
```
示例 4：
```
输入：n = 2, pick = 2
输出：2
``` 

提示：
- 1 <= n <= 2^31 - 1
- 1 <= pick <= n

## 题解

解题思路：使用二分搜索，调用guess函数，来判断中间元素是否是目标值

解题步骤：
1. 从数组的中间元素开始，如果中间元素正好是目标值，则搜索过程结束。
2. 如果目标值大于或者小于中间元素，则在数组大于或小于中间元素的那一半中查找。

### 解法1 分治


#### 1. 解题思路
- 1. 二分搜索，同样具备“分、解、合”的特性。
- 2. 考虑选择分而治之。

解题步骤：
- 1. 分:计算中间元素，分割数组。
- 2. 解:递归地在较大或者较小子数组进行二分搜索。
- 3. 合:不需要此步，因为在子数组中搜到就返回了。


#### 2. 代码实现
```js
var guessNumber = function(n) {
    const rec = (low, high) => {
        if(low>high) return;
        const mid = Math.floor((low + high) / 2);
        const res = guess(mid);
        if(res === 0) {
            return mid;
        } else if(res === 1){
            return rec(mid + 1, high);
        } else {
            return rec(1, mid - 1);
        }
    }
    return rec(1, n)
};
```

#### 3. 复杂度分析
时间复杂度O(logN),空间复杂度O(logN)(递归调用堆栈)

### 解法2 迭代法

#### 1. 代码实现
```js
var guessNumber = function(n) {
    let low = 1;
    let high = n;
    while(low <= high) {
        const mid = Math.floor((low + high) / 2);
        const res = guess(mid);
        if(res === 0) {
            return mid;
        } else if(res === 1){
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
};
```

#### 2. 复杂度分析
时间复杂度O(logN), 空间复杂度：O(1)