## 题目描述

原题地址：[最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)  

难度：简单

给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。

注意：如果 s 中存在这样的子串，我们保证它是唯一的答案。

示例 1：
```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
```
示例 2：
```
输入：s = "a", t = "a"
输出："a"
```

提示：
- 1 <= s.length, t.length <= 10^5
- s 和 t 由英文字母组成

## 解题
#### 1. 解题思路

1. 先找出所有的包含T的子串
2. 找出长度最小的那个子串，返回即可

解题步骤：

1. 用双指针维护一个滑动窗口
2. 移动右指针，找到包含T的子串，移动左指针，尽量减少包含T的字串的长度
3. 循环上述过程，找出包含T的最小子串


#### 2. 代码实现
```js
var minWindow = function(s, t) {
    let l = 0;
    let r = 0;
    const need = new Map();
    for(let c of t) {
        need.set(c, need.has(c) ? need.get(c) + 1 : 1)
    }
    let needType = need.size;
    let res = ''
    while(r < s.length) {
        const c = s[r]
        if(need.has(c)){
            need.set(c, need.get(c) - 1);
            if(need.get(c) === 0) needType -= 1
        }
        while(needType === 0) {
            const newRes = s.substring(l, r + 1)
            if(!res || newRes.length < res.length) res = newRes
            const c2 = s[l]
            if(need.has(c2)) {
                need.set(c2, need.get(c2) + 1)
                if(need.get(c2) === 1) needType += 1
            }
            l += 1
        }
        r += 1;
    }
    return res;
};
```

#### 3. 复杂度分析
时间复杂度O(m+n), m是t的长度，n是s的长度
空间复杂度O(m)

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/minimum-window-substring/solution/zui-xiao-fu-gai-zi-chuan-by-leetcode-solution/)