## 题目描述

原题地址：[电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

**难度：中等**

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![](./img/17_telephone_keypad.png)
 
示例 1：
```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```
示例 2：
```
输入：digits = ""
输出：[]
```
示例 3：
```
输入：digits = "2"
输出：["a","b","c"]
```

提示：

0 <= digits.length <= 4
digits[i] 是范围 ['2', '9'] 的一个数字。

#### 1. 题解思路
考虑使用回溯算法，套用代码模版
```js
result = [];
function backtrack (path, list) {
    if (满足条件) {
        result.push(path);
        return
    }
    
    for () {
        // 做选择(前序遍历)
        backtrack (path, list)
        // 撤销选择(后续遍历)
    }
}
```

解题步骤：
1. 用递归模拟出所有情况。
2. 收集所有到达递归终点的情况，并返回。

#### 2. 代码实现
```js
var letterCombinations = function(digits) {
    const combinations = [];
    if (digits.length == 0) {
        return combinations;
    }
    const phoneMap = new Map();
    phoneMap.set('2', "abc");
    phoneMap.set('3', "def");
    phoneMap.set('4', "ghi");
    phoneMap.set('5', "jkl");
    phoneMap.set('6', "mno");
    phoneMap.set('7', "pqrs");
    phoneMap.set('8', "tuv");
    phoneMap.set('9', "wxyz");

    let combination = '';

    backtrack(combinations, phoneMap, digits, 0, combination);
    return combinations;

    function backtrack(combinations, phoneMap, digits, index, combination) {
        if (index == digits.length) {
            combinations.push(combination);
        } else {
            let digit = digits.charAt(index);
            let letters = phoneMap.get(digit);
            for (let i = 0, lettersCount = letters.length; i < lettersCount; i++) {
                combination += letters.charAt(i);
                backtrack(combinations, phoneMap, digits, index + 1, combination);
                combination = combination.substring(0, index);
            }
        }
    }
};
```

## 高赞题解
[官方题解](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/solution/dian-hua-hao-ma-de-zi-mu-zu-he-by-leetcode-solutio/)  
[回溯+队列 图解](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/solution/hui-su-dui-lie-tu-jie-by-ml-zimingmeng/)