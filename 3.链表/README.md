原文地址：https://juejin.cn/post/6885524352465551367

## 1. 什么是链表

### 1.1 链表产生的原因

数组缺点：
1. 数组的大小是固定的，并且数组需要连续的内存空间。
2. 从数组的起点或中间插入或移除项的成本很高，因为需要移动元素。

链表解决了数组的上述问题，链表不需要连续的内存，链表中的元素可存储再内存的任何地方，如下图

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1008a07c1777426993d55dd0250b6701~tplv-k3u1fbpfcp-watermark.image)

并且链表添加或移除元素的时候不需要移动其它元素，只需要更改next 的指向即可。

### 1.2 链表特点
- 多个元素组成的列表。
- 元素存储不连续,用next指针连在一起。

下图是一个简单的链表
![](//p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f32bacd002694417be0868809fcd7782~tplv-k3u1fbpfcp-zoom-1.image)

```js
const a = { val: 'a' };
const b = { val: 'b' };
const c = { val: 'c' };
const d = { val: 'd' };
a.next = b;
b.next = c;
c.next = d;

// 遍历链表
let p = a;
while (p) {
    console.log(p.val);
    p = p.next;
}

// 在c和d中间插入e
const e = { val: 'e' };
c.next = e;
e.next = d;

// 删除e
c.next = d;
```

### 1.3 链表与数组区别
- 数组:增删非首尾元素时往往需要移动元素。
- 链表:增删非首尾元素，不需要移动元素，只需要更改next 的指向即可。

### 1.4 链表缺点
相比数组，链表也有以下缺点

- 链表需要指针，在数组中，我们可以直接访问任何位置的任何元素，而想要访问链表中间的一个元素，需要从表头开始迭代链表直到找到所需的元素。

## 2. 模拟链表操作

javascript本身没有链表，所以我们用js来模拟一个链表。

一个链表包含如下常用方法

>push(element)：向链表尾部添加一个新元素。   
>insert(element, position)：向链表的特定位置插入一个新元素。  
>getElementAt(index)：返回链表中特定位置的元素。如果链表中不存在这样的元素，则返回 undefined。    
>remove(element)：从链表中移除一个元素。  
>indexOf(element)：返回元素在链表中的索引。如果链表中没有该元素则返回-1。  
>removeAt(position)：从链表的特定位置移除一个元素。  
>isEmpty()：如果链表中不包含任何元素，返回 true，如果链表长度大于 0则返回 false。   
>size()：返回链表包含的元素个数，与数组的 length 属性类似。  
>toString()：返回表示整个链表的字符串。由于列表项使用了 Node 类，就需要重写继承自 JavaScript 对象默认的 toString 方法，让其只输出元素的值。  

```js
class Node {
    constructor(element) {
      this.element = element
      this.next = null
    }
}

class LinkedList {
    constructor () {
        this.count = 0
        this.head = null
    }

    // 向链表尾部添加新元素
    push(element) {
        const node = new Node(element)
        let current
        if (this.head === null) {
            this.head = node
        } else {
            current = this.head
            while (current.next !== null) { // 获得最后一项
                current = current.next
            }
            // 将其 next 赋为新元素，建立连接
            current.next = node
        }
        this.count++
    }
    
    // 返回链表中特定位置的元素
    getElementAt(index) {
        if (index >= 0 && index <= this.count) {
            let node = this.head
            for (let i = 0; i < index && node !== null; i++) {
                node = node.next
            }
            return node
        }
        return null
    }

    // 从链表中移除元素
    removeAt (index) {
        if (index >= 0 && index < this.count) {
            let current = this.head
            if (index === 0) {
                this.head = current.next
            } else {
                const previous = this.getElementAt(index - 1)
                current = previous.next
                // 将 previous 与 current 的下一项链接起来：跳过 current，从而移除它
                previous.next = current.next
            }
            this.count--
            return current.element
        }
        return null
    }

    // 插入元素
    insert(element, index) {
        if (index >= 0 && index <= this.count) {
            const node = new Node(element)
            if (index === 0) {
                const current = this.head
                node.next = current
                this.head = node
            } else {
                const previous = this.getElementAt(index - 1)
                node.next = previous.next
                previous.next = node
            }
            this.count++
            return true
        }
        return false
    }

    // 返回某个链表位置
    indexOf(element) {
        let current = this.head
        for (let i = 0; i < this.size() && current !== null; i++) {
            if (element === current.element) {
                return i
            }
            current = current.next
        }
        return -1
    }

    // 从链表中移除元素
    remove(element) {
        const index = this.indexOf(element)
        return this.removeAt(index)
    }

    isEmpty() {
        return this.size() === 0
    }

    size() {
        return this.count
    }

    getHead() {
        return this.head
    }

    clear() {
        this.head = null
        this.count = 0
    }

    toString() {
        if (this.head === null) {
            return ''
        }
        let objString = `${this.head.element}`
        let current = this.head.next
        for (let i = 1; i < this.size() && current !== null; i++) {
            objString = `${objString},${current.element}`
            current = current.next
        }
        return objString
    }
}

const list = new LinkedList()
list.push(15)
list.push(10)
console.log(list.toString())
```

推荐文章：  
[链表代码实现分析](https://www.geeksforgeeks.org/implementing-a-linked-list-in-java-using-class/)   
[Java中链表实现](http://developer.classpath.org/doc/java/util/LinkedList-source.html)

## 3. 复杂度分析
- prepend: O(1)
- append: O(1)
- 查找(lookup): O(n)
- 插入(insert): O(1)
- 删除(delete): O(1)

## 4. 双向链表
双向链表和普通链表的区别在于，在链表中， 一个节点只有链向下一个节点的链接;而在双向链表中，链接是双向的:一个链向下一个元素，另一个链向前一个元素，如下图所示:

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/17b43c1f8a1242afbb8a94fe5da9818d~tplv-k3u1fbpfcp-watermark.image)

双向链表代码实现：
```js
class DoublyNode extends Node {
    constructor(element, next, prev) {
        super(element, next);
        this.prev = prev;
    }
}
class DoublyLinkedList extends LinkedList {
    constructor() {
        super();
        this.tail = undefined;
    }
    
    push(element) {
        const node = new DoublyNode(element);
        if (this.head == null) {
            this.head = node;
            this.tail = node; // NEW
        } else {
            this.tail.next = node;
            node.prev = this.tail;
            this.tail = node;
        }
        this.count++;
    }
    
    // 向任意位置插入元素
    insert(element, index) {
        if (index >= 0 && index <= this.count) {
            const node = new DoublyNode(element); 
            let current = this.head;
            if (index === 0) { // 在头部插入
                //  如果双向链表为空，head 和 tail 都指向这个新节点
                if (this.head == null) {
                    this.head = node;
                    this.tail = node;
                } else {
                    node.next = this.head;
                    current.prev = node;
                    this.head = node;
                }
            } else if (index === this.count) { // 尾部插入
                current = this.tail;
                current.next = node;
                node.prev = current;
                this.tail = node;
            } else { // 中间插入
                const previous = this.getElementAt(index - 1);
                current = previous.next;
                node.next = current;
                previous.next = node;
                current.prev = node;
                node.prev = previous;
            } 
            this.count++; 
            return true;
        }
        return false;
    }
    
    // 从任意位置移除元素
    removeAt(index) {
        if (index >= 0 && index < this.count) {
            let current = this.head; 
            if (index === 0) { // 删除头部
                this.head = current.next;
                // 如果只有一项，更新tail
                if (this.count === 1) {
                    this.tail = undefined; 
                } else {
                    this.head.prev = undefined;
                }
            } else if (index === this.count - 1) { // 删除尾部
                current = this.tail;
                this.tail = current.prev;
                this.tail.next = undefined;
            } else { // 删除中间
                current = this.getElementAt(index);
                const previous = current.prev;
                // 将previous与current的下一项链接起来——跳过current 
                previous.next = current.next;
                current.next.prev = previous;
            }
            this.count--;
            return current.element;
        }
        return undefined;
    }
}
```

起点插入新元素：
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/34df9d8956794dc8b095b4e64252ada1~tplv-k3u1fbpfcp-watermark.image)

尾部插入元素：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/839b4d48eb4d4fb8a3e6010a1e41db82~tplv-k3u1fbpfcp-watermark.image)

中间插入元素：
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6f5839ce2339418a87c226b97e0efb0f~tplv-k3u1fbpfcp-watermark.image)



## 5. 循环链表
**循环链表**可以像链表一样只有单向引用，也可以像双向链表一样有双向引用。循环链表和链表之间唯一的区别在于，最后一个元素指向下一个元素的指针(tail.next)不是引用undefined，而是指向第一个元素(head)

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4e3e25921ce74a42a96b81b02516a7d9~tplv-k3u1fbpfcp-watermark.image)

双向循环链表有指向 head 元素的 tail.next 和指向 tail 元素的 head.prev
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/63ca51f08cf5414cbc73232c11131007~tplv-k3u1fbpfcp-watermark.image)

循环链表代码实现
```js
class CircularLinkedList extends LinkedList {
    constructor() {
        super();
    }

    push(element) {
        const node = new Node(element);
        let current;
        if (this.head == null) {
            this.head = node;
        } else {
            current = this.getElementAt(this.size() - 1);
            current.next = node;
        }
        // 指向头部
        node.next = this.head;
        this.count++;
    }

    insert(element, index) {
        if (index >= 0 && index <= this.count) {
            const node = new Node(element);
            let current = this.head;
            if (index === 0) { // 头部插入
                if (this.head == null) { // 链表为空
                    this.head = node;
                    node.next = this.head;
                } else {
                    node.next = current;
                    current = this.getElementAt(this.size());
                    this.head = node;
                    current.next = this.head;
                }
            } else { // 其余位置插入
                const previous = this.getElementAt(index - 1);
                node.next = previous.next;
                previous.next = node;
            }
            this.count++;
            return true;
        }
        return false;
    }

    removeAt(index) {
        if (index >= 0 && index < this.count) {
            let current = this.head;
            if (index === 0) {
                if (this.size() === 1) { // 只有一个元素情况
                    this.head = undefined;
                } else {
                    const removed = this.head;
                    current = this.getElementAt(this.size() - 1);
                    this.head = this.head.next;
                    current.next = this.head;
                    current = removed;
                }
             } else {
                const previous = this.getElementAt(index - 1);
                current = previous.next;
                previous.next = current.next;
            }
            this.count--;
            return current.element;
        }
        return undefined;
    }
}
```

链表为空时头部插入
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e84c5196eae44289b51b4782b96a8907~tplv-k3u1fbpfcp-watermark.image)

链表不为空时头部插入
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b8d8b2d1fbad47838c66b3eed16828a9~tplv-k3u1fbpfcp-watermark.image)

## 6. 有序链表
**有序链表**是指保持元素有序的链表结构。除了使用排序算法之外，我们还可以将元素插入到正确的位置来保证链表的有序性

有序链表代码实现：
```js
const Compare = {
  LESS_THAN: -1,
  BIGGER_THAN: 1,
  EQUALS: 0
}

function defaultCompare(a, b) {
  if (a === b) {
    return Compare.EQUALS;
  }
  return a < b ? Compare.LESS_THAN : Compare.BIGGER_THAN;
}
```

```js
class SortedLinkedList extends LinkedList {
    constructor(compareFn = defaultCompare) {
        super();
        this.compareFn = compareFn;
    }

    push(element) {
        if (this.isEmpty()) {
            super.push(element);
        } else {
            const index = this.getIndexNextSortedElement(element);
            super.insert(element, index);
        }
    }

    insert(element, index = 0) {
        if (this.isEmpty()) {
            return super.insert(element, index === 0 ? index : 0);
        }
        const pos = this.getIndexNextSortedElement(element);
        return super.insert(element, pos);
    }
    
    // 获取插入位置
    getIndexNextSortedElement(element) {
        let current = this.head;
        let i = 0;
        for (; i < this.size() && current; i++) {
            const comp = this.compareFn(element, current.element);
            if (comp === Compare.LESS_THAN) {
                return i;
            }
            current = current.next;
        }
        return i;
    }
}
```

## 7. 使用双向链表创建栈结构
之所以使用双向链表而不是链表，是因为对栈来说，我们会向链表尾部添加元素，也会从链表尾部移除元素。DoublyLinkedList 类有列表最后一个元素的引用，无须迭代整个链表的元素就能获取它。双向链表可以直接获取头尾的元素，减少过程消耗，它的时间复杂度和原始的 Stack 实现相同，为 O(1)。
```js
class StackLinkedList {
    constructor() {
        this.items = new DoublyLinkedList();
    }

    push(element) {
        this.items.push(element);
    }

    pop() {
        if (this.isEmpty()) {
            return undefined;
        }
        const result = this.items.removeAt(this.size() - 1);
        return result;
    }

    peek() {
        if (this.isEmpty()) {
            return undefined;
        }
        return this.items.getElementAt(this.size() - 1).element;
    }

    isEmpty() {
        return this.items.isEmpty();
    }

    size() {
        return this.items.size();
    }

    clear() {
        this.items.clear();
    }

    toString() {
        return this.items.toString();
    }
}
```

## 8. 跳表
### 8.1 什么是跳表
跳表是链表的优化，跳表在工程中主要在redis中进行运用

跳表的优化思想：
- 升维、空间换时间
- 添加更多指针(头、尾、中...指针)

原始链表
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e958430f8950419ea4049e58c907a6c2~tplv-k3u1fbpfcp-watermark.image)
查询时间复杂度O(n)

添加索引
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3bee4c9506d6438ca071f9177bf806f1~tplv-k3u1fbpfcp-watermark.image)
增加一级索引，可以看到，一级索引当问速度为2n，二级索引访问速度为4n，以此类推，增加(log2n级)多级索引

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fed12cae48404e0ea705d995399ebb08~tplv-k3u1fbpfcp-watermark.image)

### 8.2 跳表的时间复杂度
n/2、n/4、 n/8、第k级索弓结点的个数就是n/(2^k)

假设索引有h级，最高级的索引有2个结点。n/(2^h)=2,从而求得 h=log2(n)-1

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9e91c207b5be4ddfa5284fddada6018d~tplv-k3u1fbpfcp-watermark.image)
索引的高度: logn, 每层索引遍历的结点个数: 3，在跳表中查询任意数据的时间复杂度就是0(logn)


### 8.3 现实中跳表的形态
在现实中，由于元素的增加和删除，导致有些所以并不是完全非常工整的，经过多次改动之后，有些地方会多跨，有些地方会少跨， 索引的增加和删除时间复杂度为logn

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c091312c77c14feb9e80560b4d37ef86~tplv-k3u1fbpfcp-watermark.image)

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/990af84cd204443cab3944ff232df1b3~tplv-k3u1fbpfcp-watermark.image)

### 8.4 工程中的应用
[LRU Cache - Linked list](https://leetcode-cn.com/problems/lru-cache/)  
[Redis - Skip List](http://redisbook.readthedocs.io/en/latest/internal-datastruct/skiplist.html)  
[为啥 redis 使用跳表(skiplist)而不是使用 red-black？](https://www.zhihu.com/question/20202931)


## 9. 常见链表问题
### 1. 原型链
- 原型链的本质是链表。
- 原型链上的节点是各种原型对象，比如Function.prototype、Object.prototype.....
- 原型链通过 `__proto__` 属性连接各种原型对象。
```
obj -> Object.prototype -> null
func -> Function.prototype -> Object.prototype -> null
arr -> Array.prototype -> Object.prototype -> null
```

**原型链知识点**：如果A沿着原型链能找到`B.prototype`, 那么 `A instanceof B` 为 true.

```js
const obj = {}
obj instanceof Object // true
```
如果在A对象上没有找到x属性，那么会沿着原型链找x属性

**instanceof原理，并用代码实现**

- 知识点:如果A沿着原型链能找到B.prototype,那么A instanceof B为true。
- 解法: 遍历A的原型链，如果找到B.prototype,返回true,否则返回false
```js
const instanceOf = (A, B) => {
    let p = A;
    while(p) {
        if(p === B.prototype) {
            return true;
        }
        p = p.__proto__
    }
    return false;
}
```

```
var foo = [],
F = function() {};
Object.prototype.a = 'value a';
Function.prototype.b = 'value b';

console.log(foo.a);
console.log(foo.b);

console.log(F.a);
console.log(F.b);
```

### 2. 使用链表指针获取JSON的节点值
```js
const json = {
    a: { b: { c: 1 } },
    d: { e: 2 },
};

const path = ['a', 'b', 'c'];

let p = json;
path.forEach((k) => {
    p=p[k];
});
```
- 链表里的元素存储不是连续的，之间通过 next 连接
- JavaScript中没有链表，但可以用Object模拟链表。
- 链表常用操作:修改next、遍历链表。
- JS 中的原型链也是一一个链表。
- 使用链表指针可以获取JSON的节点值。

## 10. leetcode常见题解

### [删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

**难度：简单**

解题思路：将被删除节点转移到下个节点

解题步骤：将被删节点的值改为下个节点的值，然后删除下个节点
```js
var deleteNode = function(node) {
    node.val = node.next.val
    node.next = node.next.next
};
```

### [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)  

**难度：简单**

题解：[反转链表(递归法+迭代法)](https://leetcode-cn.com/problems/reverse-linked-list/solution/fan-zhuan-lian-biao-di-gui-fa-die-dai-fa-41ee/)

### [环形链表](https://leetcode-cn.com/problems/linked-list-cycle/) 

**难度：简单**

题解：[环形链表(快慢指针)](https://leetcode-cn.com/problems/linked-list-cycle/solution/huan-xing-lian-biao-kuai-man-zhi-zhen-by-zf1b/)

### [删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

**难度：简单**

题解：[删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/solution/shan-chu-pai-xu-lian-biao-zhong-de-zhong-hdk4/)

### [两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

**难度：中等**

题解：[两数相加](https://leetcode-cn.com/problems/add-two-numbers/solution/liang-shu-xiang-jia-by-zxhnext-ixs1/)

## 11. 双指针问题
### [爬楼梯](https://leetcode.com/problems/climbing-stairs/)  

**难度：简单**

题解：[爬楼梯(双指针)](https://leetcode-cn.com/problems/climbing-stairs/solution/pa-lou-ti-shuang-zhi-zhen-by-zxhnext-f075/)

### [移动0](https://leetcode-cn.com/problems/move-zeroes/)   

**难度：简单**

题解：[移动零(双指针)](https://leetcode-cn.com/problems/move-zeroes/solution/yi-dong-ling-shuang-zhi-zhen-by-zxhnext-t27s/)

### [盛水最多的容器](https://leetcode-cn.com/problems/container-with-most-water/)  

**难度：中等**

题解：[盛水最多的容器(双指针)](https://leetcode-cn.com/problems/container-with-most-water/solution/sheng-shui-zui-duo-de-rong-qi-shuang-zhi-lhsn/)

### [(高频老题）三数之和](https://leetcode-cn.com/problems/3sum/)  

**难度：中等**

题解：[三数之和(排序+双指针)](https://leetcode-cn.com/problems/3sum/solution/san-shu-zhi-he-pai-xu-shuang-zhi-zhen-by-dkq7/)
