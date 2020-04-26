双指针（对撞指针、快慢指针）

快慢 指针, 用于删除排序好的重复数组

如: [1,2,3,3,4,5]

i=0;
j=1;

```js
const removeDuplicates = (arr) => {
  let i = 0;
  let j = 1;
  for (let j = 1; j < arr.length; j++) {
    if (arr[j] !== arr[i]) {
      i++;
      arr[i] = arr[j];
    }
  }
  console.log(i + 1);
};
removeDuplicates([1, 1, 2, 2, 3, 3, 4]);
```

[]

与此相似一道题

[1,2,3,5] 3

[1,2,5,5]

```js
function removeElement(nums, val) {
  let i = 0;
  for (let j = 0; j < nums.length; j++) {
    if (nums[j] != val) {
      nums[i] = nums[j];
      i++;
    }
  }
  console.log(nums);
  return i;
}
```

## 寻找缺失的数字

示例 1：

输入：[3,0,1]
输出：2

示例 2：

输入：[9,6,4,2,3,5,7,0,1]
输出：8

示例 3：

输入：[1,2]
输出：0

解法 Two 循环

```js
function missingNumber(arr) {
  let min = 0;

  for (let i = 0; i < arr.length + 1; i++) {
    let flag = false;
    for (let value of arr) {
      if (min === value) {
        flag = true;
        break;
      }
    }
    if (!flag) {
      return min;
    }
    min++;
  }
}
```

构建 hash 循环

牺牲一定的空间,换取时间

```js
function missingNumber(arr) {
  let hash = {};
  for (let i = 0; i < arr.length; i++) {
    hash[arr[i]] = i;
  }

  for (let i = 0; i < arr.length + 1; i++) {
    if (hash[i] === undefined) {
      return i;
    }
  }
}
```

````js

  /*
        给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。
        最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。
        你可以假设除了整数 0 之外，这个整数不会以零开头。
        示例 1:
        输入: [1,2,3]
        输出: [1,2,4]
        解释: 输入数组表示数字 123。
        示例 2:

        输入: [4,3,2,1]
        输出: [4,3,2,2]
        解释: 输入数组表示数字 4321。

      */

var plusOne = function (digits) {
        if (digits.length === 1) {
          digits[0] = digits[0] + 1;
          if (digits[0] === 10) {
            return [1, 0];
          }
          return digits;
        }
        const base = plusOne(digits.slice(1));
        if (digits.length === base.length) {
          if (digits[0] + 1 === 10) {
            return [1, 0, ...base.slice(1)];
          } else {
            return [digits[0] + 1, ...base.slice(1)];
          }
        }

        return [digits[0], ...base];
      };
      ```
````

## 判断 两个给定的二叉树是否相等

此题,我采用遍历树(有中序遍历,后续遍历,以及先序遍历),然后加入数组中,比较...

应该还有解法,不用遍历树两遍

```js
const p = {
  val: 1,
  left: {
    val: 2,
    left: null,
    right: null,
  },
  right: null,
};

const q = {
  val: 1,
  left: null,
  right: {
    val: 2,
    left: null,
    right: null,
  },
};
var isSameTree = function (p, q) {
  let tree1 = [];
  let tree2 = [];
  helper(p, tree1);
  helper(q, tree2);

  return tree1.toString() === tree2.toString();
};
const helper = (tree, result) => {
  if (tree === null) {
    result.push("null");
    return;
  }
  const left = helper(tree.left, result);
  const right = helper(tree.right, result);
  result.push(tree.val);
};

// 是在是妙~~~~~~~~~~,完全没有想到

function isSameTree(p, q) {
  // p and q are both null
  if (p == null && q == null) return true;
  // one of p and q is null
  if (q == null || p == null) return false;
  if (p.val != q.val) return false;
  return isSameTree(p.right, q.right) && isSameTree(p.left, q.left);
}

console.log(isSameTree(p, q));
```
