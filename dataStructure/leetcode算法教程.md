# LeetCode 算法教程

## 1 算法入门

[20 天算法刷题计划](https://leetcode-cn.com/study-plan/algorithms/?progress=w8l6fp6)

### 1.1  二分查找

#### 例题

- 简单的二分查找

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

- 第一个错误的版本

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

- 搜索插入位置

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

#### 二分查找  简单题

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

- #### [第 k 个缺失的正整数](https://leetcode-cn.com/problems/kth-missing-positive-number/)

```js
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {number}
 */
var findKthPositive = function(arr, k) {
    // 找规律，先找范围，范围是 x_n - n, n 是下标值，结果是 k + n
    let left = 0, right = arr.length - 1
    while(left <= right) {
        let mid = left + ((right - left) >> 1)
        let temp = arr[mid] - mid - 1
        if(temp > k) {
            right = mid - 1
        } else if(temp < k) {
            left = mid + 1
        } else {
            while(arr[mid] - arr[mid-1] == 1) mid = mid -1      // 找到最小的下标
            left = mid
            break
        }
    }
    return k + left
};
```

- #### [特殊数组的特征值](https://leetcode-cn.com/problems/special-array-with-x-elements-greater-than-or-equal-x/)

这题的规律不好找啊，溜了溜了，就做了前三页的简单题

### 1.2  双指针

#### 例题

- 有序数组的平方

为什么没有用 JavaScript 内置的方法，反而时间和空间的消耗更大了呢

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortedSquares = function(nums) {
    // 1. 先用暴力法
    // nums.forEach((value, index) => nums[index] = value * value)
    // return nums.sort((a, b) => a - b)

    // 2. 应该用一种简单的方法，平方之后前面是降序，后面是升序，归并一下即可
    nums.forEach((value, index) => nums[index] = value * value)
    let res = []
    for(let i = 0, j = nums.length - 1; i <= j;) {
        if(nums[i] <= nums[j]) {
            res.unshift(nums[j])
            --j;
        } else {
            res.unshift(nums[i])
            ++i;
        }
    }
    return res
};
```

- 轮转数组

三种方法，感觉还不如用 JavaScript 自带的内置函数，这样速度还要快一点

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var reverse = function(arr, i, j) {
    for(;i < j;++i, --j) {
        let temp = arr[i]
        arr[i] = arr[j]
        arr[j] = temp
    }
}

var rotate = function(nums, k) {
    // 1. 利用 splice
    // k = k % nums.length
    // let temp = nums.splice(0, nums.length - k)
    // for(let i of temp) nums.push(i)


    // 2. 就是快排的基础, 环状的替换, 感觉有点不好找规律，，，
    // k = k % nums.length
    // let m = 0, n = nums.length - k, temp
    // for(let i = 0; i < k; ++i) {
    //     [nums[m], nums[n]] = [nums[n], nums[m]]
    //     ++m 
    //     ++n
    // }
    // while(m < nums.length - k) 

    // 3. 使用翻转的方法
    k = k % nums.length
    nums.reverse()
    reverse(nums, 0, k - 1)
    reverse(nums, k, nums.length - 1)
};
```

- 移动零

```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function(nums) {
    // 两个指针，一个 firstZero，一个指针用来遍历数组
    let firstZero = -1, temp
    for(let i = 0; i < nums.length; ++i) {
        if(nums[i] == 0) {
            if(firstZero == -1) firstZero = i
        } else {
            if(firstZero != -1) {
                temp = nums[i]
                nums[i] = nums[firstZero]
                nums[firstZero] = temp
                if(firstZero + 1 < nums.length && nums[firstZero + 1] == 0) firstZero++
                else firstZero = i
            }
        }
    }
};
```

- 两数之和

```js
/**
 * @param {number[]} numbers
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(numbers, target) {
    // 双指针了
    let i = 0, j = numbers.length - 1
    while(i < j && numbers[i] + numbers[j] != target) {
        if(numbers[i] + numbers[j] > target) {
            --j 
        } else {
            ++i 
        }
    }
    return [i + 1, j + 1]
};
```

- 反转字符串

```js
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
var reverseString = function(s) {
    // 1. 直接使用内置的方法
    // s.reverse()

    // 2. 使用双指针
    let temp
    for(let i = 0, j = s.length - 1; i < j; ++i, --j) {
        temp = s[i]
        s[i] = s[j]
        s[j] = temp
    }
};
```

- 反转字符串中的单词

```js
/**
 * @param {string} s
 * @return {string}
 */
var reverseWords = function(s) {
    // 双指针
    let firstSpace = 0, secondSpace = 0, temp
    s = s.split('')
    while(s[secondSpace] != ' ' && secondSpace < s.length) ++secondSpace
    while(secondSpace <= s.length) {
        for(let i = firstSpace, j = secondSpace - 1; i < j; ++i, --j) {
            temp = s[i]
            s[i] = s[j]
            s[j] = temp
        }
        secondSpace++
        firstSpace = secondSpace
        while(s[secondSpace] != ' ' && secondSpace < s.length) ++secondSpace
    }
    return s.join('')
};
```

- 链表的中间结点

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var middleNode = function(head) {
    let firstNode = head, secondNode = head
    while(secondNode && secondNode.next) {
        if(secondNode.next) secondNode = secondNode.next.next
        firstNode = firstNode.next
    }
    return firstNode
};
```

- 删除链表的倒数第 N 个结点

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    let first = head, second = head
    for(let i = 0; i < n && second; ++i) second = second.next
    if(!second) return head.next    // 处理长度一致的尴尬
    while(second && second.next) {
        first = first.next
        second = second.next
    }
    first.next = first.next.next
    return head
};
```

