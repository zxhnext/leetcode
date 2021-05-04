## 题目描述

原题地址：[无重复字符的最长字串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

难度： 中等

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:
```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
示例 2:
```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
示例 3:
```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```
示例 4:
```
输入: s = ""
输出: 0
```

提示：
- 0 <= s.length <= 5 * 10^4
- s 由英文字母、数字、符号和空格组成

## 题解
#### 1. 解题思路
1. 先找出所有的不包含重复字符的字串
2. 找出长度最大的哪个子串，返回其长度即可

解题步骤：
1. 用双指针维护一个滑动窗口，用来剪切子串
2. 不断移动右指针，遇到重复字符，就把左指针移动到重复字符的下一位
3. 过程中，记录所有窗口的长度，并返回最大值

#### 2. 代码实现
```js
var lengthOfLongestSubstring = function(s) {
    let start = 0;
    let res = 0;
    const map = new Map()
    for(let end = 0; end < s.length; end++) {
        if(map.has(s[end]) && map.get(s[end]) >= start) {
            start = map.get(s[end]) + 1
        }
        res = Math.max(res, end - start + 1)
        map.set(s[end], end)
    }
    return res
};
```

#### 3. 复杂度分析
时间复杂度O(n), 空间复杂度O(m),字符串中不重复字符串的个数

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/wu-zhong-fu-zi-fu-de-zui-chang-zi-chuan-by-leetc-2/)  
[画解算法：3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/hua-jie-suan-fa-3-wu-zhong-fu-zi-fu-de-zui-chang-z/)  