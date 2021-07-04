## 题目描述

原题地址：[反转字符串](https://leetcode-cn.com/problems/reverse-string/)

**难度：简单**

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。


示例 1：
```
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```
示例 2：
```
输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```

## 题解
#### 1. 解题思路
反转后的字符串前半部分等于反转前的后半部分，后半部分等于反转前的前半部分，所以我们可以使用左右指针，将关于中点对称的元素的值调换即可。

#### 2. 代码实现
```js
var reverseString = function(s) {
    let left = 0, right = s.length - 1;
    while (left < right) {
        [s[left], s[right]] = [s[right], s[left]];
        left++;
        right--;
    }
    return s;
};
```

**ts版**
```ts
function reverseString(s: string[]): void {
    let left = 0, right = s.length -1;
    while (left < right) {
        [s[left], s[right]] = [s[right], s[left]];
        left++;
        right--;
    }
};
```

#### 3. 复杂度分析
时间复杂度O(n)，空间复杂度O(1)