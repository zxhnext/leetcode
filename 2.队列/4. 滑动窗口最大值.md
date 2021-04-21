## 原题地址
[柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

难度：困难
## 题解
### 题解1 使用栈
#### 1. 问题分析
对于没有闭合的左括号而言，越靠后的左括号，对应的右括号越靠前`{[]}`，满足后进先出，考虑用栈  

#### 2. 解题思路
1. 新建一个栈
2. 扫描字符串，遇左括号入栈，遇到和栈顶括号类型匹配的右括号就出栈，类型不匹配直接判定为不合法。 
3. 最后栈空了就合法，否则不合法

```js
var isValid = function(s) {
    if(s.length % 2 === 1) { return false; }
    const stack = []
    for(let i = 0; i < s.length; i += 1) {
        const c = s[i]
        if(c === '(' || c === '{' || c === '[') {
            stack.push(c)
        } else {
            const t = stack[stack.length - 1]
            if(
                (t === '(' && c === ')') || 
                (t === '{' && c === '}') || 
                (t === '[' && c === ']')
            ) {
                 stack.pop()
            } else {
                return false
            }
        }
    }
    return stack.length === 0
};
```

#### 3.使用字典优化
使用字典优化逻辑判断

```js
var isValid = function(s) {
	if(s.length % 2 === 1) { return false }
	const stack = []
	const map = new Map()
	map.set('(', ')')
	map.set('{', '}')
	map.set('[', ']')
	for(let i = 0; i < s.length; i++) {
		const c = s[i]
		if(map.has(c)) {
			stack.push(c)
		} else {
			const t = stack[stack.length - 1]
			if(map.get(t) === c) {
				stack.pop()
			} else {
				return false
			}
		}
	}
	return stack.length === 0
};
```
#### 4. 复杂度分析
时间复杂度O(n)，空间复杂度：O(n)
## 高赞题解
[官方题解](https://leetcode-cn.com/problems/valid-parentheses/solution/you-xiao-de-gua-hao-by-leetcode-solution/)  
[辅助栈法，极简+图解](https://leetcode-cn.com/problems/valid-parentheses/solution/valid-parentheses-fu-zhu-zhan-fa-by-jin407891080/)  
[逐步分析，图解栈](https://leetcode-cn.com/problems/valid-parentheses/solution/zhu-bu-fen-xi-tu-jie-zhan-zhan-shi-zui-biao-zhun-d/)  
