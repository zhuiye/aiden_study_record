## hash Table

与此相对应的实现为 Map

hash 表一种 散列的数据结构,利用它能快速查找到想要的数据

时间复杂度为 O(n),它的本质是 根据 key 值,通过 一个函数(叫做 hash 函数)生成一个值(这个数叫做 hash 值)

根据定义,js 简单实现的一个 hash 表如下:

```js
class HashTable {
  constructor() {
    this.item = {};
  }

  set(key, value) {
    this.item[this.hashFunction(key)] = {
      [key]: value,
    };
  }

  get(key) {
    const code = this.hashFunction(key);
    if (code) {
      return this.item[code][key];
    }
    return undefined;
  }
}

function hashFunction(key) {
  return "返回code,可以自行去查找一些hash函数";
}
```

看了,上面肯定有疑惑了,为什么要特意生成一个 code 值呢,而不是直接 this.item[key]

当然还要考虑 hash 表 键值 碰撞问题,有分离链法,和线性探索法,这里就不过多的描述了

关于 hash 表更生动的描述,[请点击->](https://www.zhihu.com/question/330112288/answer/744362539)
