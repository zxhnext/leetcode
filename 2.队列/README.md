原文地址： [前端必会数据结构与算法系列之队列](https://juejin.cn/post/6882588628187349000)

## 1. 什么是队列

队列是遵循先进先出（FIFO，也称为先来先服务）原则的一组有序的项。队列在尾部添加新元素，并从顶部移除元素。最新添加的元素必须排在队列的末尾。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f5599cafee3a485a99761c5efb293187~tplv-k3u1fbpfcp-watermark.webp)

队列的插入与删除(因为只能在队头和队尾操作)都是O(1)操作，查询操作是O(n)

## 2. 模拟一个队列
javascript本身没有队列，所以我们用js来模拟一个队列的实现。

一个常用的队列包含如下方法：
> enqueue(element(s))：向队列尾部添加一个（或多个）新的项。  
> dequeue()：移除队列的第一项（即排在队列最前面的项）并返回被移除的元素。  
> peek()：返回队列中第一个元素——最先被添加，也将是最先被移除的元素。队列不做任何变动（不除元素，只返回元素信息——与 Stack 类的 peek 方法非常类似）。该方法在其他语言中也可以叫作 front 方法。  
> isEmpty()：如果队列中不包含任何元素，返回 true，否则返回 false。   
> size()：返回队列包含的元素个数，与数组的 length 属性类似  

同栈一样，对于队列而言任何表的实现都是合法的。

### 2.1 数组实现

数组是元素的一个有序集合，为了保证元素排列有序，它会占用更多的内存空间。

当用数组实现时，对于每一个队列数据结构，我们保留一个数组 Queue[] 以及位置 Front 和Rear，它们代表队列的两端。我们还要记录实际存在于队列中的元素的个数 Size。下图表示处于某个中间状态的一个队列。顺便指出，图中那些空白单元是有着不确定的值的。特别地，前三个单元含有曾经属于该队列的元素。

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e1242e70d9b24414906380b79fda6d38~tplv-k3u1fbpfcp-watermark.image)

我们发现一个问题，当经过 10 次入队后队列似乎是满了，因为 Rear 现在是 10，而下一次再人队就会是一个不存在的位置。然而，队列中也许只存在几个元素，因为若干元素可能已经出队了。像栈一样，即使在有许多操作的情况下队列也常常不是很大。
简单的解决方法是，只要 Front 或Rear 到达数组的尾端，它就又绕回到开头。下图显示在某些操作期间的队列情况。这叫做循环数组（circular array）实现。
实现回绕所需要的附加代码是极小的（虽然它可能使得运行时间加倍）。如果 Fronr 或 Rear 增1使得超越了数组，那么其值就要重置为数组的第一个位置。

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a073fe24e23e41049608b512051aa559~tplv-k3u1fbpfcp-watermark.image)

另外，数组的大部分方法的时间复杂度为O(n)，每次修改数组的index，都相当于把下一个位置的元素拷贝到当前位置，一直到最后一个index为止，所以数组的性能较差，这里不再实现。

### 2.2 用对象实现
```js
class Queue { 
  constructor() { 
    this.count = 0
    this.lowestCount = 0
    this.items = {}
  } 

  // 向队列尾部添加一个（或多个）新的项
  enqueue(element){
    this.items[this.count] = element
    this.count++
  }

  // 移除队列的第一项（即排在队列最前面的项）并返回被移除的元素
  dequeue() {
    if (this.isEmpty()) { 
      return undefined
    } 
    const result = this.items[this.lowestCount]
    delete this.items[this.lowestCount]
    this.lowestCount++
    return result
  }

  // 返回队列中第一个元素, 队列不做任何变动
  peek() {
    if (this.isEmpty()) { 
      return undefined
    } 
    return this.items[this.lowestCount]
  }

  // 如果队列中不包含任何元素，返回 true，否则返回 false
  isEmpty() {
    return this.count - this.lowestCount === 0
  }

  // 返回队列包含的元素个数，与数组的 length 属性类似
  size() {
    return this.count - this.lowestCount
  }

  clear() { 
    this.items = {}
    this.count = 0
    this.lowestCount = 0
  }

  toString() { 
    if (this.isEmpty()) { 
      return ''
    } 
    let objString = `${this.items[this.lowestCount]}`
    for (let i = this.lowestCount + 1; i < this.count; i++) { 
      objString = `${objString},${this.items[i]}`
    } 
    return objString
  }
}
```

### 2.3 链表实现
尝试用链表实现

JAVA queqe源码 http://fuseyism.com/classpath/doc/java/util/Queue-source.html

## 3. 双端队列
双端队列（deque，或称 double-ended queue）是一种允许我们同时从前端和后端添加和移除元素的特殊队列。

一般工程中都用双端队列，相当于栈和队列的结合版，如下图所示：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fb15497f82534766849573b59f4b206a~tplv-k3u1fbpfcp-watermark.image)

双端队列在现实生活中的例子有电影院、餐厅中排队的队伍等。举个例子，一个刚买了票的人如果只是还需要再问一些简单的信息，就可以直接回到队伍的头部。另外，在队伍末尾的人如果赶时间，他可以直接离开队伍。

双端队列的一个常见应用是存储一系列的撤销操作。每当用户在软件中进行了一个操作，该操作会被存在一个双端队列中（就像在一个栈里）。当用户点击撤销按钮时，该操作会被从双端队列中弹出，表示它被从后面移除了。在进行了预先定义的一定数量的操作后，最先进行的操作会被从双端队列的前端移除。由于双端队列同时遵守了先进先出和后进先出原则，可以说它是把队列和栈相结合的一种数据结构

### 3.1 实现一个双端队列
```js
class Deque { 
  constructor() { 
    this.count = 0
    this.lowestCount = 0
    this.items = {}
  }

  // 该方法在双端队列前端添加新的元素
  addFront(element) {
    if (this.isEmpty()) { 
      this.addBack(element)
    } else if (this.lowestCount > 0) {
      this.lowestCount-- 
      this.items[this.lowestCount] = element 
    } else { 
      for (let i = this.count; i > 0; i--) {
        this.items[i] = this.items[i - 1]
      } 
      this.count++ 
      this.items[0] = element
    }
  }

  // 该方法在双端队列后端添加新的元素
  addBack(element) {
    this.items[this.count] = element
    this.count++
  }

  // 该方法会从双端队列前端移除第一个元素
  removeFront() {
    if (this.isEmpty()) { 
      return undefined
    } 
    const result = this.items[this.lowestCount]
    delete this.items[this.lowestCount]
    this.lowestCount++
    return result
  }

  // 该方法会从双端队列后端移除第一个元素
  removeBack() {
    if (this.isEmpty()) { 
      return undefined
    } 
    this.count--
    const result = this.items[this.count]
    delete this.items[this.count]
    return result
  }

  // 该方法返回双端队列前端的第一个元素
  peekFront() {
    if (this.isEmpty()) { 
      return undefined
    } 
    return this.items[this.lowestCount]
  }

  // 该方法返回双端队列后端的第一个元素
  peekBack() {
    if (this.isEmpty()) { 
      return undefined
    } 
    return this.items[this.count - 1]
  }

  // 如果队列中不包含任何元素，返回 true，否则返回 false
  isEmpty() {
    return this.count - this.lowestCount === 0
  }

  // 返回队列包含的元素个数，与数组的 length 属性类似
  size() {
    return this.count - this.lowestCount
  }

  clear() { 
    this.items = {}
    this.count = 0
    this.lowestCount = 0
  }
}
```

## 4. 优先队列
- 1.插入操作: O(1)
- 2.取出操作: O(logN) - 按照元素的优先级取出
- 3.底层具体实现的数据结构较为多样和复杂: heap、 bst、 treap

JAVA 优先队列实现 https://docs.oracle.com/javase/10/docs/api/java/util/PriorityQueue.html

## 5. 常见队列问题
### 5.1 循环队列——击鼓传花游戏
在这个游戏中，孩子们围成一个圆圈，把花尽快地传递给旁边的人。某一时刻传花停止，这个时候花在谁手里，谁就退出圆圈、结束游戏。重复这个过程，直到只剩一个孩子（胜者）

```js
function hotPotato(elementsList, num) {
    const queue = []
    const elimitatedList = []
    for(let i = 0; i < elementsList.length; i++) {
        queue.unshift(elementsList[i])
    }

    while (queue.length > 1) {
        for (let i = 0; i < num; i++) {
            queue.unshift(queue.pop())
        }
        elimitatedList.push(queue.pop())
    }

    return {
        eliminated: elimitatedList, 
        winner: queue.pop()
    }
}

const names = ['John', 'Jack', 'Camila', 'Ingrid', 'Carl']
const result = hotPotato(names, 7)

result.eliminated.forEach(name => { 
    console.log(`${name}在击鼓传花游戏中被淘汰。`)
}); 
console.log(`胜利者： ${result.winner}`)
```

过程如下图：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b607fa15d41a41f4b50ac1a836ee8347~tplv-k3u1fbpfcp-watermark.image)
### 5.2 回文检查器
回文是正反都能读通的单词、词组、数或一系列字符的序列，例如 madam或 racecar，下面的算法使用了一个双端队列来解决问题

```js
function palindromeChecker(aString) {
  if (aString === undefined || aString === null || (aString !== null && aString.length === 0)) {
    return false;
  }
  const deque = new Deque();
  const lowerString = aString.toLocaleLowerCase().split(' ').join('');
  let isEqual = true;
  let firstChar, lastChar;
  for (let i = 0; i < lowerString.length; i++) {
    deque.addBack(lowerString.charAt(i));
  }
  while (deque.size() > 1 && isEqual) { 
    firstChar = deque.removeFront();
    lastChar = deque.removeBack();
    if (firstChar !== lastChar) {
      isEqual = false;
    }
  }
  return isEqual;
}
```

## 6. leetcode常见考题

### [最近的请求次数](https://leetcode-cn.com/problems/number-of-recent-calls/)

**难度：简单**

### [用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

**难度：简单**


### [设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)

**难度：中等**

### [设计循环双端队列](https://leetcode-cn.com/problems/design-circular-deque/?utm_source=LCUS&utm_medium=ip_redirect&utm_campaign=transfer2china)

**难度：中等**

### [滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

**难度：困难**

以上题解请参考：[leetcode刷题之路](https://github.com/zxhnext/leetcode/tree/master/2.%E9%98%9F%E5%88%97)

持续更新ing...