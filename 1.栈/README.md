原文地址：[前端必会数据结构与算法系列之栈](https://juejin.cn/post/6874869558662922248)

## 1. 什么是栈
栈是一种遵从后进先出（LIFO）原则的有序集合。新添加或待删除的元素都保存在栈的同一端，称作栈顶，另一端就叫栈底。在栈里，新元素都靠近栈顶，旧元素都接近栈底

如下图所示：  

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ae1788c53d3493c8c1581f1cf6f605e~tplv-k3u1fbpfcp-zoom-1.image)

## 2. 模拟一个栈

javascript本身没有栈，所以我们用js来模拟一个栈的实现。

一个栈包含如下方法：
> push(element(s))：添加一个（或几个）新元素到栈顶。  
> pop()：移除栈顶的元素，同时返回被移除的元素。    
> peek()：返回栈顶的元素，不对栈做任何修改（不会移除栈顶的元素，仅仅返回它）。   
> isEmpty()：如果栈里没有任何元素就返回 true，否则返回 false。     
> clear()：移除栈里的所有元素。  
> size()：返回栈里的元素个数。该方法和数组的 length 属性很类似。  

### 2.1 数组实现
我们尝试用数组实现一个栈。
```js
class Stack { 
  constructor() { 
    this._items = [];
  }

  push(element) { 
    this._items.push(element); 
  }

  pop() { 
    return this._items.pop(); 
  }

  peek() { 
    return this._items[this._items.length - 1]; 
  }

  isEmpty() { 
    return this._items.length === 0; 
  }

  clear() { 
    this._items = []; 
  }

  size() { 
    return this._items.length; 
  }
}
```

### 2.2 对象实现
```js
class Stack { 
  constructor() { 
    this._count = 0; 
    this._items = {}; 
  } 

  push (element) { 
    this._items[this._count] = element; 
    this._count++; 
  }

  pop() { 
    if (this.isEmpty()) {
      return undefined; 
    } 
    this._count--;
    const result = this._items[this._count];
    delete this._items[this._count];
    return result;
  }

  peek() { 
    if (this.isEmpty()) { 
      return undefined; 
    } 
    return this._items[this._count - 1]; 
  }

  isEmpty() { 
    return this._count === 0; 
  }

  clear() { 
    this._items = {}; 
    this._count = 0; 
    // LIFO 原则
    // while (!this.isEmpty()) { 
	//	 this.pop(); 
    // }
  }

  size() { 
    return this._count; 
  }

  toString() { 
    if (this.isEmpty()) { 
      return ''; 
    } 
    let objString = `${this._items[0]}`;
    for (let i = 1; i < this._count; i++) {
      objString = `${objString},${this._items[i]}`;
    } 
    return objString; 
  }
}
```

### 2.3 用链表实现
栈的第一种实现方法是使用单链表。我们通过在链表顶端插人来实现 push，通文删除链表顶端元素实现 pop。代码不再详述。


由于栈是一个表，因此任何实现表的方法都能实现栈。

另外可以参考下Java栈的源码实现
[JAVA stack源码](http://developer.classpath.org/doc/java/util/Stack-source.html)



### 2.4 实现栈私有属性

```js
console.log(Object.getOwnPropertyNames(stack)); // [ '_count', '_items' ]
console.log(Object.keys(stack)); // [ '_count', '_items' ]
console.log(stack.items);
```
由以上代码我们可以发现，我们自己实现的栈的内部属性是可以被访问到和修改的，但是在实际使用时，我们并不希望这样。我们希望能保护内部结构，只使用我们暴露出去的方法。

#### 2.4.1 下划线约定
```js
class Stack { 
  constructor() { 
    this._count = 0; 
    this._items = {}; 
  } 
}
```
这种方式只是一种约定，并不能保护数据，而且只能依赖于使用我们代码的开发者所具备的常识

#### 2.4.2 WeakMap
```js
const _items = new WeakMap();
const _count = new WeakMap();

class Stack {
  constructor() {
    _count.set(this, 0);
    _items.set(this, {});
  }

  push(element) {
    const items = _items.get(this);
    const count = _count.get(this);
    items[count] = element;
    _count.set(this, count + 1);
  }

  pop() {
    if (this.isEmpty()) {
      return undefined;
    }
    const items = _items.get(this);
    let count = _count.get(this);
    count--;
    _count.set(this, count);
    const result = items[count];
    delete items[count];
    return result;
  }

  peek() {
    if (this.isEmpty()) {
      return undefined;
    }
    const items = _items.get(this);
    const count = _count.get(this);
    return items[count - 1];
  }

  isEmpty() {
    return _count.get(this) === 0;
  }

  size() {
    return _count.get(this);
  }

  clear() {
    /* while (!this.isEmpty()) {
        this.pop();
      } */
    _count.set(this, 0);
    _items.set(this, {});
  }

  toString() {
    if (this.isEmpty()) {
      return '';
    }
    const items = _items.get(this);
    const count = _count.get(this);
    let objString = `${items[0]}`;
    for (let i = 1; i < count; i++) {
      objString = `${objString},${items[i]}`;
    }
    return objString;
  }
}
```

数组版本

```js
const items = new WeakMap();
class Stack { 
    constructor () { 
        items.set(this, []);
    } 
    push(element){ 
        const s = items.get(this);
        s.push(element); 
    } 
    pop(){ 
        const s = items.get(this); 
        const r = s.pop(); 
        return r; 
    } 
    // 其他方法
}
```

WeakMap 可以确保属性是私有的，这种方法items 在 Stack 类里是真正的私有属性。但是采用这种方法，代码的可读性不强，而且在扩展该类时无法继承私有属性。

#### 2.4.3 类属性提案
TypeScript 提供了一个给类属性和方法使用的 private 修饰符。然而，该修饰符只在编译时有用。在代码被转移完成后，属性同样是公开的

## 3. 常见的栈问题
### 3.1 从十进制到二进制
> 后出来的余数反而要排到前面。  
> 把余数依次入栈，然后再出栈,就可以实现余数倒序输出  

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/69f08a57a1334d628d96d43d2e142114~tplv-k3u1fbpfcp-zoom-1.image)
```js
const Stack = require('./objectStack.js')

function decimalToBinary(decNumber) {
  const remStack = new Stack();
  let number = decNumber; 
  let rem;
  let binaryString = '';
  while(number > 0) {
    rem = Math.floor(number % 2)
    remStack.push(rem)
    number = Math.floor(number / 2)
  }
  while (!remStack.isEmpty()) {
    binaryString += remStack.pop().toString(); 
  } 
  return binaryString;
}

console.log(decimalToBinary(233)); // 11101001 
console.log(decimalToBinary(10)); // 1010 
console.log(decimalToBinary(1000)); // 1111101000
console.log(decimalToBinary(1000.99)); // 1111101000
```

### 3.2 十进制转任意进制
```js
const Stack = require('./objectStack.js')

function baseConverter(decNumber, base) {
  const remStack = new Stack()
  const digits = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ'
  let number = decNumber
  let rem;
  let binaryString = '';
  if(!(base >= 2 && base <= 36)) { return '' }
  while(number > 0) {
    rem = Math.floor(number % base)
    remStack.push(rem)
    number = Math.floor(number / base)
  }
  while(!remStack.isEmpty()) {
    binaryString += digits[remStack.pop()]
  }
  return binaryString;
}

console.log(baseConverter(100345, 2)); // 11000011111111001 
console.log(baseConverter(100345, 8)); // 303771 
console.log(baseConverter(100345, 16)); // 187F9 
console.log(baseConverter(100345, 35)); // 2BW0
```

### 3.3 函数调用堆栈
 最后调用的函数，最先执行完。  
 JS解释器使用栈来控制函数的调用顺序。

### 3.4 回文检查器
回文是正反都能读通的单词、词组、数或一系列字符的序列，例如 madam或 racecar
```js
var isPalindrome = function(x) {
    let startString = x + ''
    if (startString === undefined || startString === null || (startString !== null && startString.length === 0)) {
        return false
    }

    const stack = []
    let i = 0
    let lowerString = ''
    while(i < startString.length) {
        stack.push(startString[i++])
    }

    while(stack.length) {
        lowerString += stack.pop()
    }

    return lowerString === startString
};
```

## 4. leetcode栈考题
### 有效的括号
原题地址：[有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

**难度：简单**


### 最小栈
原题地址：[最小栈](https://leetcode-cn.com/problems/min-stack/)

**难度：简单**

### 用栈实现队列
原题地址：[用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

**难度：简单**

### 用两个栈实现队列
原题地址：[用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

**难度：简单**

### 二叉树的前序遍历

原题地址：[二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

**难度：中等**

### 每日温度
原题地址：[每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

**难度：中等**
### 柱状图中最大的矩形
原题地址:[柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

**难度：困难**

### 接雨水
原题地址: [接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

**难度：困难**


