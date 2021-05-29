原文地址： https://juejin.cn/post/6885534619039596557  
集合与字典知识详解：https://juejin.cn/post/6962523376448045093

## 1. 散列表
### 1.1 散列函数
散列函数是这样的函数，即无论你给它什么数据，它都还你一个数字。散列算法的作用是尽可能快地在数据结构中找到一个值，散列算法的作用是尽可能快地在数据结构中找到一个值。散列函数的作用是给定一个键值，然后返回值在表中的地址。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c299073c909d4324968c28a7ba1ea516~tplv-k3u1fbpfcp-watermark.image)

专业术语：将输入映射到数字

要求：
1. 同样的输入应该由同样的输出。
2. 它应将不同的输入映射到不同的数字(最理想的情况，不同的输入输出都不相同)

我们可以使用散列函数和数组创建一个散列表，散列函数将不同的输入映射到不同的索引。

如下图

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d83252441c69434f8c1966532dfbed6f~tplv-k3u1fbpfcp-watermark.image)

以一个电子邮件地址簿为例，来看一个最常见的散列函数
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0a74eff54d5348f0a12ca5fa51905cf8~tplv-k3u1fbpfcp-watermark.image)

### 1.2 冲突
实际上不可能有不同的输入输出都不相同的散列函数。有时候，一些键会有相同的散列值。不同的值在散列表中对应相同位置的时候，我们称其为冲突(如果给两个输入分配位置相同，就出现了冲突)。

这时会在该位置存储一个链表。如下图：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cbf0b3bbe2cf4120bd8661cdaccb67db~tplv-k3u1fbpfcp-watermark.image)


## 2. 散列表
散列表其实就是散列函数 + 数组。

散列表是字典的一种实现，JavaScript 语言内部就是使用散列表来表示每个对象。此时，对象的每个属性和方法(成员)被存储为 key 对象类型，每个 key 指向对应的对象成员。


### 2.1 实现一个散列表

- put(key,value)：向散列表增加一个新的项（也能更新散列表）。
- remove(key)：根据键值从散列表中移除值。
- get(key)：返回根据键值检索到的特定的值。

公共方法：
```js
defaultToString(item) {
  if (item === null) {
    return 'NULL';
  } else if (item === undefined) {
    return 'UNDEFINED';
  } else if (typeof item === 'string' || item instanceof String) {
    return `${item}`;
  }
  return item.toString();
}

class ValuePair {
  constructor(key, value) {
    this.key = key;
    this.value = value;
  }
  toString() {
    return `[#${this.key}: ${this.value}]`;
  }
}
```

```js
class HashTable {
  constructor(toStrFn = defaultToString) {
    this.toStrFn = toStrFn;
    this.table = {};
  }
  
  // 散列函数
  loseloseHashCode(key) {
    if (typeof key === 'number') { // 如果是数字，直接返回
      return key;
    }
    // 将 key 转换为一个字符串，防止 key 是一个对象而不是字符串
    const tableKey = this.toStrFn(key);
    let hash = 0;
    // 将从 ASCII 表中查到的每个字符对应的 ASCII 值加到 hash 变量中
    for (let i = 0; i < tableKey.length; i++) {
      hash += tableKey.charCodeAt(i);
    }
    // 使用 hash 值和一个任意数做除法的余数，这可以规避操作数超过数值变量最大表示范围的风险。
    return hash % 37;
  }

  hashCode(key) {
    return this.loseloseHashCode(key);
  }

  put(key, value) {
    if (key != null && value != null) {
      const position = this.hashCode(key);
      this.table[position] = new ValuePair(key, value);
      return true;
    }
    return false;
  }

  get(key) {
    const valuePair = this.table[this.hashCode(key)];
    return valuePair == null ? undefined : valuePair.value;
  }

  remove(key) {
    const hash = this.hashCode(key);
    const valuePair = this.table[hash];
    if (valuePair != null) {
      delete this.table[hash];
      return true;
    }
    return false;
  }

  getTable() {
    return this.table;
  }

  isEmpty() {
    return this.size() === 0;
  }

  size() {
    return Object.keys(this.table).length;
  }

  clear() {
    this.table = {};
  }

  toString() {
    if (this.isEmpty()) {
      return '';
    }
    const keys = Object.keys(this.table);
    let objString = `{${keys[0]} => ${this.table[keys[0]].toString()}}`;
    for (let i = 1; i < keys.length; i++) {
      objString = `${objString},{${keys[i]} => ${this.table[keys[i]].toString()}}`;
    }
    return objString;
  }
}
```

### 2.2 处理散列表中的冲突
上面讲散列函数时已经说过。有时候，一些键会有相同的散列值。不同的值在散列表中对应相同位置的时候，我们称其为冲突。

#### 2.2.1 分离链接法
分离链接法包括为散列表的每一个位置创建一个链表并将元素存储在里面。它是解决冲突的最简单的方法，但是在 HashTable 实例之外还需要额外的存储空间。如下图所示：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1782bfb31585469aabdca8d06d489245~tplv-k3u1fbpfcp-watermark.image)

```js
class HashTableSeparateChaining {
  constructor(toStrFn = defaultToString) {
    this.toStrFn = toStrFn;
    this.table = {};
  }

  loseloseHashCode(key) {}

  hashCode(key) {}

  put(key, value) {
    if (key != null && value != null) {
      const position = this.hashCode(key);
      // 如果是第一次向该位置加入元素，我们会在该位置上初始化一个 LinkedList 类的实例
      if (this.table[position] == null) {
        this.table[position] = new LinkedList();
      }
      this.table[position].push(new ValuePair(key, value));
      return true;
    }
    return false;
  }

  get(key) {
    const position = this.hashCode(key);
    const linkedList = this.table[position];
    if (linkedList != null && !linkedList.isEmpty()) {
      let current = linkedList.getHead();
      while (current != null) {
        if (current.element.key === key) {
          return current.element.value;
        }
        current = current.next;
      }
    }
    return undefined;
  }

  remove(key) {
    const position = this.hashCode(key);
    const linkedList = this.table[position];
    if (linkedList != null && !linkedList.isEmpty()) {
      let current = linkedList.getHead();
      while (current != null) {
        if (current.element.key === key) {
          linkedList.remove(current.element);
          if (linkedList.isEmpty()) {
            delete this.table[position];
          }
          return true;
        }
        current = current.next;
      }
    }
    return false;
  }

  getTable() {}

  isEmpty() {}

  size() {
    let count = 0;
    Object.values(this.table).forEach(linkedList => {
      count += linkedList.size();
    });
    return count;
  }

  clear() {}
 
  toString() {}
```

#### 2.2.2 线性探查

当想向表中某个位置添加一个新元素的时候，如果索引为 position 的位置已经被占据了，就尝试 position+1 的位置。如果 position+1 的位置也被占据了，就尝试 position+2 的位置，以此类推，直到在散列表中找到一个空闲的位置

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/91c60198c07e46c89831ce9848fab8fb~tplv-k3u1fbpfcp-watermark.image)

当我们从散列表中移除一个键值对的时候，如果我们只是移除了元素，就可能在查找有相同 hash（位置）的其他元素时找到一个空的位置，这会导致算法出现问题

线性探查技术分为两种。第一种是软删除方法。我们使用一个特殊的值（标记）来表示键值对被删除了（惰性删除或软删除），而不是真的删除它。经过一段时间，散列表被操作过后，我们会得到一个标记了若干删除位置的散列表。这会逐渐降低散列表的效率，因为搜索键值会随时间变得更慢。下图展示了这个过程。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/834453c9718b4fb088060119257f7c62~tplv-k3u1fbpfcp-watermark.image)

第二种方法需要检验是否有必要将一个或多个元素移动到之前的位置。当搜索一个键的时候，这种方法可以避免找到一个空位置。如果移动元素是必要的，我们就需要在散列表中挪动键值对。下图展现了这个过程

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0de200a32d67448396a0d4966a0360f3~tplv-k3u1fbpfcp-watermark.image)

方法2实现代码
```js
class HashTableLinearProbing {
  constructor(toStrFn = defaultToString) {
    this.toStrFn = toStrFn;
    this.table = {};
  }
  loseloseHashCode(key) {}

  hashCode(key) {}

  put(key, value) {
    if (key != null && value != null) {
      const position = this.hashCode(key);
      if (this.table[position] == null) {
        this.table[position] = new ValuePair(key, value);
      } else {
        let index = position + 1;
        // 找到下一个没有被占据的位置
        while (this.table[index] != null) {
          index++;
        }
        this.table[index] = new ValuePair(key, value);
      }
      return true;
    }
    return false;
  }

  get(key) {
    const position = this.hashCode(key);
    if (this.table[position] != null) {
      // 要找的值是否就是原始位置上的值
      if (this.table[position].key === key) {
        return this.table[position].value;
      }
      
      let index = position + 1;
      while (this.table[index] != null && this.table[index].key !== key) {
        index++;
      }
      // 是否是我们要找的键
      if (this.table[index] != null && this.table[index].key === key) {
        return this.table[position].value;
      }
    }
    return undefined;
  }

  remove(key) {
    const position = this.hashCode(key);
    if (this.table[position] != null) {
      if (this.table[position].key === key) {
        delete this.table[position];
        this.verifyRemoveSideEffect(key, position);
        return true;
      }
      let index = position + 1;
      while (this.table[index] != null && this.table[index].key !== key) {
        index++;
      }
      if (this.table[index] != null && this.table[index].key === key) {
        delete this.table[index];
        this.verifyRemoveSideEffect(key, index);
        return true;
      }
    }
    return false;
  }
  
  // 验证删除操作是否有副作用(不同位置上是否存在具有相同 hash 的元素)
  verifyRemoveSideEffect(key, removedPosition) {
    const hash = this.hashCode(key);
    let index = removedPosition + 1;
    // 直到找到一个空位置
    while (this.table[index] != null) {
      const posHash = this.hashCode(this.table[index].key);
      if (posHash <= hash || posHash <= removedPosition) {
        // 将当前元素移动至 removedPosition 的位置
        this.table[removedPosition] = this.table[index];
        delete this.table[index];
        removedPosition = index;
      }
      index++;
    }
  }
  
  isEmpty() {}

  size() {}

  clear() {}
  
  getTable() {}
  
  toString() {}
}
```

方法1实现代码(惰性删除)
```js
class HashTableLinearProbingLazy {
  constructor(toStrFn = defaultToString) {
    this.toStrFn = toStrFn;
    this.table = {};
  }

  loseloseHashCode(key) {}
  
  hashCode(key) {}

  put(key, value) {
    if (key != null && value != null) {
      const position = this.hashCode(key);
      if (
        this.table[position] == null ||
        (this.table[position] != null && this.table[position].isDeleted)
      ) {
        this.table[position] = new ValuePairLazy(key, value);
      } else {
        let index = position + 1;
        while (this.table[index] != null && !this.table[position].isDeleted) {
          index++;
        }
        this.table[index] = new ValuePairLazy(key, value);
      }
      return true;
    }
    return false;
  }
  
  get(key) {
    const position = this.hashCode(key);
    if (this.table[position] != null) {
      if (this.table[position].key === key && !this.table[position].isDeleted) {
        return this.table[position].value;
      }
      let index = position + 1;
      while (
        this.table[index] != null &&
        (this.table[index].key !== key || this.table[index].isDeleted)
      ) {
        if (this.table[index].key === key && this.table[index].isDeleted) {
          return undefined;
        }
        index++;
      }
      if (
        this.table[index] != null &&
        this.table[index].key === key &&
        !this.table[index].isDeleted
      ) {
        return this.table[position].value;
      }
    }
    return undefined;
  }
  
  remove(key) {
    const position = this.hashCode(key);
    if (this.table[position] != null) {
      if (this.table[position].key === key && !this.table[position].isDeleted) {
        this.table[position].isDeleted = true;
        return true;
      }
      let index = position + 1;
      while (
        this.table[index] != null &&
        (this.table[index].key !== key || this.table[index].isDeleted)
      ) {
        index++;
      }
      if (
        this.table[index] != null &&
        this.table[index].key === key &&
        !this.table[index].isDeleted
      ) {
        this.table[index].isDeleted = true;
        return true;
      }
    }
    return false;
  }

  isEmpty() {}
  
  size() {
    let count = 0;
    Object.values(this.table).forEach(valuePair => {
      count += valuePair.isDeleted === true ? 0 : 1;
    });
    return count;
  }
  
  clear() {}
  
  getTable() {}
  
  toString() {}
}
```

### 2.3 更好的散列函数
```js
djb2HashCode(key) {
    const tableKey = this.toStrFn(key);
    let hash = 5381;
    for (let i = 0; i < tableKey.length; i++) {
        hash = (hash * 33) + tableKey.charCodeAt(i);
    }
    return hash % 1013;
}
```
