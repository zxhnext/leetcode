## 题目描述

原题地址：[有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

难度：简单

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

示例 1:
```
输入: s = "anagram", t = "nagaram"
输出: true
```
示例 2:
```
输入: s = "rat", t = "car"
输出: false
```
说明:
你可以假设字符串只包含小写字母。

进阶:
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

## 题解
#### 题解1 排序
#### 1. 解题思路
1. 如果两个字符串长度不同，肯定不是异位词。
2. 将两个字符串排序后比较是否相同。

#### 2. 代码实现
```js
var isAnagram = function(s, t) {
    if (s.length !== t.length) return false;
    return [...s].sort().join('') === [...t].sort().join('');
};
```

#### 3. 复杂度分析
时间复杂度：O(nlogn)，其中 n 为 s 的长度。排序的时间复杂度为 O(nlogn)，比较两个字符串是否相等时间复杂度为 O(n)，因此总体时间复杂度为 O(nlogn+n)=O(nlogn)。

空间复杂度：O(logn)。排序需要 O(logn) 的空间复杂度

### 题解2 hash表
#### 1. 解题思路
维护一个26位hash表table，初始每项值都为0，遍历字符串s,记录每个字符的频次，然后遍历t,减去对应字母的频次，如果出现table[i]<0，则说明 t 包含一个不在 s 中的额外字符，返回 false 即可

#### 2. 代码实现
```js
var isAnagram = function(s, t) {
    if (s.length !== t.length) {
        return false;
    }
    const table = new Array(26).fill(0);
    for (let i = 0; i < s.length; ++i) {
        table[s.codePointAt(i) - 'a'.codePointAt(0)]++;
    }
    for (let i = 0; i < t.length; ++i) {
        table[t.codePointAt(i) - 'a'.codePointAt(0)]--;
        if (table[t.codePointAt(i) - 'a'.codePointAt(0)] < 0) {
            return false;
        }
    }
    return true;
};
```

#### 3. 复杂度分析
时间复杂度：O(n)，其中 n 为 s 的长度。

空间复杂度：O(S)，其中 S 为字符集大小，此处 S=26

## 高赞题解
[画解算法：242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/solution/hua-jie-suan-fa-242-you-xiao-de-zi-mu-yi-wei-ci-by/)