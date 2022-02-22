# LeetCode 算法教程

## 算法入门

[20 天算法刷题计划](https://leetcode-cn.com/study-plan/algorithms/?progress=w8l6fp6)

### 第一天

#### 二分查找

考虑数组的奇偶数，应该向下取整，平时用 c 做应该就是向下取整了，使用 JavaScript  的时候没有类型，应该使用  Math.floor 或者 Math.ceil 向下或者向上取整

我的代码：

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
    if(nums.length === 0) return -1;
    let mid = Math.floor(nums.length/2)
    if(nums[mid] === target) {
        return mid;
    } else if(nums[mid] > target) {
        return search(nums.splice(0, mid), target)
    } else {
        let res = search(nums.splice(mid + 1, nums.length), target)
        if(res === -1) return -1
        else return mid + res + 1;
    }
};
```

官方代码：

```js
var search = function(nums, target) {
    let low = 0, high = nums.length - 1;
    while (low <= high) {
        const mid = Math.floor((high - low) / 2) + low;
        const num = nums[mid];
        if (num === target) {
            return mid;
        } else if (num > target) {
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return -1;
};
```

递归的写法和循环的写法用时差不多，但是循环的内存好像要少一点。比较两者而言，我还是喜欢递归的写法