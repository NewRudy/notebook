# LeetCode 算法教程

## 算法入门

[20 天算法刷题计划](https://leetcode-cn.com/study-plan/algorithms/?progress=w8l6fp6)

### 第一天

#### 二分查找

考虑数组的奇偶数，应该向下取整，平时用 c 做应该就是向下取整了，使用 JavaScript  的时候没有类型，应该使用  Math.floor 或者 Math.ceil 向下或者向上取整

- left 和 right 怎么取，如果是数组，left 是 0， right 是数组长度减一，其它的又是另外的情况了
- 循环的判断条件是 left < right 还是 left <= right ，这个又怎么考虑？ <= 是为了慢慢的缩小范围，< 是为了找到一个确切的值
- 判断完之后 mid 是否要加一还是减一呢

代码：

```js
var search = function(nums, target) {
    let left = 0, right = nums.length - 1
    while(left <= right) {
        let mid = ((right - left) >> 1) + left
        if(target == nums[mid]) {
            return mid
        } else if(target < nums[mid]) {
            right = mid - 1
        } else {
            left = mid + 1
        }
    }
    return -1
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

#### 第一个错误的版本

顺序的版本，找到第一个错误的版本，最直观的就是循环，算法就是二分

代码：

```js
/**
 * @param {function} isBadVersion()
 * @return {function}
 */
var solution = function(isBadVersion) {
    /**
     * @param {integer} n Total versions
     * @return {integer} The first bad version
     */
    return function(n) {
        let left = 1, right = n
        while(left < right) {
            let mid = ((right - left) >> 1) + left
            if(isBadVersion(mid)) {
                right = mid
            } else {
                left = mid + 1
            }
        }
        return left
    };
};
```

官方代码：

```js
var solution = function(isBadVersion) {
    return function(n) {
        let left = 1, right = n;
        while (left < right) { // 循环直至区间左右端点相同
            const mid = Math.floor(left + (right - left) / 2); // 防止计算时溢出
            if (isBadVersion(mid)) {
                right = mid; // 答案在区间 [left, mid] 中
            } else {
                left = mid + 1; // 答案在区间 [mid+1, right] 中
            }
        }
        // 此时有 left == right，区间缩为一个点，即为答案
        return left;
    };
};
```

用时都差不多，但是看到题解里面有说用移位 >>1 代替除以 2 的，>>1 相当于除以2并向下取整了，防止溢出

#### 搜索插入位置

这个题的判断条件有出入，可能是最后一个位置加一，所以多用了一个变量 ans = nums.length

代码：

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var searchInsert = function(nums, target) {
    let left = 0, right = nums.length - 1, ans = nums.length
    while(left <= right) {
        let mid = ((right - left) >> 1) + left
        if(target <= nums[mid]) {
            ans = mid 
            right = mid - 1
        } else {
            left = mid + 1
        }
    }
    return ans
};
```

官方代码：

```js
var searchInsert = function(nums, target) {
    const n = nums.length;
    let left = 0, right = n - 1, ans = n;
    while (left <= right) {
        let mid = ((right - left) >> 1) + left;
        if (target <= nums[mid]) {
            ans = mid;
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return ans;
};
```

#### 相关简单题

- [x的平方根](https://leetcode-cn.com/problems/sqrtx/)

非负整数 x 的算术平方根也是二分查找吗？是的，范围是从 1 到 x，找到哪个判断条件

```js
/**
 * @param {number} x
 * @return {number}
 */
var mySqrt = function(x) {
    let left = 1, right = x
    while(left <= right) {
        let mid = left + ((right - left)>>1)
        if(mid*mid <= x) {
            if((mid+1) * (mid+1) > x) {
                return mid 
            }
            left = mid + 1
        } else {
            right = mid -1
        }
    }
    return 0
};
```

- [有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)

同上

```js
/**
 * @param {number} num
 * @return {boolean}
 */
var isPerfectSquare = function(num) {
    let left = 1, right = num 
    while(left <= right) {
        let mid = left + ((right - left) >> 1)
        let value = mid * mid
        if(value == num) {
            return true
        } else if (value < num) {
            left = mid + 1
        } else {
            right = mid -1
        }
    }
    return false
};
```

- [猜数字的大小](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

题目描述的真实太 giao 了

```js
/**
 * @param {number} n
 * @return {number}
 */
var guessNumber = function(n) {
    let left = 1, right = n 
    while(left <= right) {
        let mid = left + ((right - left) >> 1)
        let res = guess(mid)
        if(res == 0) {
            return mid
        } else if(res == -1) {
            right = mid - 1
        } else {
            left = mid + 1
        }
    }
};
```

- [排列硬币](https://leetcode-cn.com/problems/arranging-coins/)

```js
/**
 * @param {number} n
 * @return {number}
 */
var arrangeCoins = function(n) {
    let sum = 0, i = 0
    while(sum <= n){
        i = i + 1
        sum = sum + i 
    }
    return i -1
};
```

为什么这个可以用二分法呢？肯定是 1 到 n  的其中一个数满足等差数列的公式

```js
/**
 * @param {number} n
 * @return {number}
 */
var arrangeCoins = function(n) {
    let left = 1, right = n
    while(left < right) {
        let mid = left + ((right - left + 1) >> 1)
        if (mid * (mid + 1) <= 2 * n) {
            left = mid;
        } else {
            right = mid - 1;
        }
    }
    return left
};
```

- [寻找比目标字母大的最小字母](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/)

```js
/**
 * @param {character[]} letters
 * @param {character} target
 * @return {character}
 */
var nextGreatestLetter = function(letters, target) {
    let left = 0, right = letters.length
    while(left < right) {
        let mid = left + ((right - left) >> 1)
        if(target >= letters[mid]) {
            left = mid + 1
        } else {
            right = mid
        }
    }
    return letters[left] > target ? letters[left] : letters[0]
};
```

- #### [山脉数组的峰顶索引](https://leetcode-cn.com/problems/peak-index-in-a-mountain-array/)

```js
/**
 * @param {number[]} arr
 * @return {number}
 */
var peakIndexInMountainArray = function(arr) {
    let left = 0, right = arr.length - 1
    while(left < right) {
        let mid = left + ((right - left) >> 1)
        if(arr[mid] < arr[mid + 1]) {
            left = mid + 1
        } else {
            right = mid 
        }
    }
    return left
};
```

- #### [公平的糖果交换](https://leetcode-cn.com/problems/fair-candy-swap/)

这个题感觉是哈希了，不是二分法了

```js
/**
 * @param {number[]} aliceSizes
 * @param {number[]} bobSizes
 * @return {number[]}
 */
var fairCandySwap = function(aliceSizes, bobSizes) {
    const sumA = _.sum(aliceSizes), sumB = _.sum(bobSizes)
    const delta = (sumA - sumB) >> 1
    const setA = new Set(aliceSizes)
    for(let y of bobSizes) {
        let x = y + delta
        if(setA.has(x)) {
            return [x, y]
        }
    }
};
```

- #### [矩阵中战斗力最弱的 K 行](https://leetcode-cn.com/problems/the-k-weakest-rows-in-a-matrix/)

```js
/**
 * @param {number[][]} mat
 * @param {number} k
 * @return {number[]}
 */
var kWeakestRows = function(mat, k) {
    let list = []
    for(let i = 0; i < mat.length; ++i) {
        list.push({index: i, vlaue: firstZero(mat[i])})
    }
    list.sort((a, b) => a.vlaue - b.vlaue)
    return list.map((item) => item.index).splice(0, k)
};

var firstZero = function(arr) {
    let left = 0, right = arr.length - 1
    if(arr[right]) return right + 1
    while(left < right) {
        let mid = left + ((right - left) >> 1)
        if(arr[mid]) {
            left = mid + 1
        } else {
            right = mid
        }
    }
    return left
}
```

- #### [检查整数及其两倍数是否存在](https://leetcode-cn.com/problems/check-if-n-and-its-double-exist/)

是哈希表了，简单易用的就解决了

```js
/**
 * @param {number[]} arr
 * @return {boolean}
 */
var checkIfExist = function(arr) {
    let map = new Map()
    for(let i of arr) {
        if(map.has(i)) return true
        map.set(i*2, i)
        if(!(i%2)) map.set(i/2, i)
    }
    return false
};
```

- #### [统计有序矩阵中的负数](https://leetcode-cn.com/problems/count-negative-numbers-in-a-sorted-matrix/)

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var countNegatives = function(grid) {
    let row = grid.length, col = grid[0].length
    if(grid[row - 1][col - 1] >= 0) return 0
    let sum = 0
    for(let i = 0; i < row; ++i) {
        sum = sum + grid[i].length - firstNeg(grid[i])
    }
    return sum
};

var firstNeg = (arr) => {
    let left = 0, right = arr.length - 0
    if(arr[right] >= 0 ) return arr.length
    while(left < right) {
        let mid = left + ((right - left) >> 1)
        if(arr[mid] >= 0) {
            left = mid + 1
        } else {
            right = mid
        }
    }
    return left
}
```

- #### [ 两个数组间的距离值](https://leetcode-cn.com/problems/find-the-distance-value-between-two-arrays/)

```js
/**
 * @param {number[]} arr1
 * @param {number[]} arr2
 * @param {number} d
 * @return {number}
 */
var findTheDistanceValue = function(arr1, arr2, d) {
    arr2.sort((a, b) => a - b)
    console.log(arr2)
    let sum = 0
    for(let i of arr1) {
        let left = 0, right = arr2.length - 1, flag = true
        while(left <= right) {
            let mid = left + ((right - left) >> 1)
            let distance = Math.abs(arr2[mid] - i)
            if(distance <= d) {
                flag = false
                break
            }
            if(i > arr2[mid]) left = mid + 1
            else right = mid -1
        }
        if(flag){
            sum = sum + 1
        }
    }
    return sum
};
```

