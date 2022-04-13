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

用时都差不多，但是看到题解里面有说用移位 >>1 代替除以 2 的，>>1 相当于除以2并向下取整了，减了再加就是防止溢出了

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

题目描述的真是太 giao 了

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

#### 双指针 简单题

- #### [验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function(s) {
    const str = s.toLocaleLowerCase().replace(/[\W_]/ig, '')
    for(let i = 0, j = str.length - 1; i < j; ++i, --j) {
        if(str[i].toLocaleLowerCase() != str[j].toLocaleLowerCase()) return false
    }
    return true
};
```

- #### [快乐数](https://leetcode-cn.com/problems/happy-number/)

```js
/**
 * @param {number} n
 * @return {boolean}
 */
var isHappy = function(n) {
    let slow = n, fast = sumSquare(n) 
    while(fast!=1 && fast!=slow) {
        fast = sumSquare(sumSquare(fast))
        slow = sumSquare(slow)
    }
    return fast === 1
};

var sumSquare = (num) => {
    return num.toString().split('').map(i => i ** 2).reduce((a, b) => a + b)
}
```

- #### [反转字符串中的元音字母](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)

```js
/**
 * @param {string} s
 * @return {string}
 */
var reverseVowels = function(s) {
    let temp = ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U']
    const str = s.split('')
    for(let i = 0, j = str.length - 1; i < j; ++i, --j) {
        while(!temp.includes(str[i]) && i < j) ++i;
        while(!temp.includes(str[j]) && j > i) --j;
        if(i < j) [str[i], str[j]] = [str[j], str[i]]
    }
    return str.join('')
};
```

- #### [反转字符串 II](https://leetcode-cn.com/problems/reverse-string-ii/)

快慢指针是两倍的情况，可以思考是否还需要两个指针

```js
/**
 * @param {string} s
 * @param {number} k
 * @return {string}
 */
var reverseStr = function(s, k) {
    const str = s.split('')
    for(let i = 0; i <= str.length; i += 2 * k) {
        swap(str, i, Math.min(i + k, str.length) - 1)
    } 
    return str.join('')
};

var swap = (str, i, j) => {
    while(i < j) {
        [str[i], str[j]] = [str[j], str[i]]
        ++i 
        --j
    }
}
```

### 1.3 滑动窗口

[一文带你AC十道题](https://lucifer.ren/blog/2020/03/16/slide-window/)

滑动窗口是一种解决问题的思路和方法，通常用来解决一些连续的问题

- 无重复字符的最长子串

```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let str = s.split('')
    let i = 0, j = 0, max = 0
    while(j < str.length) {
        for(let k = 0; k < j - i; ++k) {        // 检查是否有重复字符
            if(str[i + k] === str[j]) {
                i = i + k + 1
                break
            }
        }
        j++
        max = j - i > max ? j - i: max 
    }
    return max
};
```

官方题解（感觉反而不好理解）：

```js
var lengthOfLongestSubstring = function(s) {
    // 哈希集合，记录每个字符是否出现过
    const occ = new Set();
    const n = s.length;
    // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
    let rk = -1, ans = 0;
    for (let i = 0; i < n; ++i) {
        if (i != 0) {
            // 左指针向右移动一格，移除一个字符
            occ.delete(s.charAt(i - 1));
        }
        while (rk + 1 < n && !occ.has(s.charAt(rk + 1))) {
            // 不断地移动右指针
            occ.add(s.charAt(rk + 1));
            ++rk;
        }
        // 第 i 到 rk 个字符是一个极长的无重复字符子串
        ans = Math.max(ans, rk - i + 1);
    }
    return ans;
};
```

- 字符串的排列

```js
/**
 * @param {string} s1
 * @param {string} s2
 * @return {boolean}
 */

var cloneMap = function(map) {
    return new Map([...map])
}

var checkInclusion = function(s1, s2) {
    if(s1.length > s2.length) return false
    let map = new Map()
    let str1 = s1.split('')
    str1.forEach( i => {        // 初始的排列的 map
        if(map.has(i)) map.set(i, map.get(i) + 1)
        else map.set(i, 1)
    })
    let str2 = s2.split('')
    let temp = cloneMap(map)
    let i = 0, j = 0, flag = false
    while(j < str2.length) {
        if(temp.has(str2[j])) {
            flag = true
            if(j - i + 1 >= str1.length) return true
            if(temp.get(str2[j]) === 1) {
                temp.delete(str2[j])
            } else {
                temp.set(str2[j], temp.get(str2[j]) - 1)
            }
            j++
        } else {
            if(flag) {
                temp = cloneMap(map)
                flag = false
            }
            i++		// 感觉自己这里的逻辑有问题
            j = i
        }
    }
    return false
};
```

他人[题解](https://leetcode-cn.com/problems/permutation-in-string/solution/hua-dong-chuang-kou-fa-zhu-shi-ji-duo-si-14ip/)（看了之后觉得自己写的是一坨:shit:）：

```js
/**
 * @param {string} s1
 * @param {string} s2
 * @return {boolean}
 */
var checkInclusion = function(s1, s2) {
    // 自定义方法，用于比较滑动窗口中的数据与目标数据是否相同
    function equal(a,b){
        // equal方法的比较思路：a和b 都是两个长度为26 的数组，数组当中的每个数据都对应着一个字符的数量
        // 例如a[0] 就代表着a 字符串当中字符'a' 的数量，a[1]就代表着字符串当中字符'b'的数量
        // 因此，只有在a,b 两个数组的每一个值都相等才能说明a,b 代表的字符串内的各个字符的数量相等，也只有在这种情况下equal 函数会返回true ，否则都会返回false
        for(let i =0;i<a.length;i++){
            if(a[i]!==b[i])return false
        }
        return true
    }


    let arrP=new Array(26).fill(0),arr=new Array(26).fill(0),len1 = s1.length,len2=s2.length
    // 将目标数据放入数组，之后使用该数组与滑动窗口进行对比
    for(let i =0;i<len1;i++) arrP[s1.charCodeAt(i)-'a'.charCodeAt()]++

    // 遍历整个长字符串，在字符串内部使用滑动窗口逐一访问判断
    for(let i =0;i<len2;i++){
        // 创建滑动窗口数组，滑动窗口数组的长度应与目标数组相同
        arr[s2.charCodeAt(i)-'a'.charCodeAt()]++
        // 滑动窗口创建完成之前不进行任何操作，长度不同不可能与目标数组相符
        if(i>=len1-1) {
            // 滑动窗口创建完毕之后即刻开始与目标窗口进行比对，比对一致即返回true
            if(equal(arr,arrP)) return true
            else{
            // 比对不一致，则滑动窗口向右滑动，最左边字符退出窗口，进入下一次循坏，在下一次循环中又会将窗口外右边的第一个字符放入窗口
            arr[s2.charCodeAt(i-len1+1)-'a'.charCodeAt()]--
            }
        }
    }
    // 比对完成都没有发现匹配数据，则说明无匹配数据，返回false
    return false
    <!-- 兄弟盟帮忙点个赞吧 -->
};
```

### 1.4 广度有限搜索/深度优先搜索

1. 图像渲染

```js
var floodFill = function(image, sr, sc, newColor) {
    dfs(image, sr, sc, newColor, image[sr][sc])
    return image
};

const dfs = (image, sr, sc, newColor, oldColor) => {
    if(sr < 0 || sr >= image.length || sc < 0 || sc >= image[0].length) return  // 越界
    if(image[sr][sc] == newColor || image[sr][sc] != oldColor) return    // 已经被上色了或者不是老的颜色
    image[sr][sc] = newColor
    dfs(image, sr, sc + 1, newColor, oldColor)
    dfs(image, sr, sc - 1, newColor, oldColor)
    dfs(image, sr + 1, sc, newColor, oldColor)
    dfs(image, sr - 1, sc, newColor, oldColor)
}
```

他人题解：

```js
const floodFill = function (image, sr, sc, newColor) {
    dfs(image, sr, sc, newColor, image[sr][sc])
    return image
};

const dfs = (image, sr, sc, newColor, originColor) => {
    // 越界
    if (sr < 0 || sr >= image.length || sc < 0 || sc >= image[0].length) return
    // 不满足上色要求, 或已经上色过
    if (image[sr][sc] != originColor || image[sr][sc] == newColor) return;
    // 上色
    image[sr][sc] = newColor

    dfs(image, sr - 1, sc, newColor, originColor)
    dfs(image, sr, sc + 1, newColor, originColor)
    dfs(image, sr + 1, sc, newColor, originColor)
    dfs(image, sr, sc - 1, newColor, originColor)
}
```

2. 岛屿的最大面积

他人[题解](https://leetcode-cn.com/problems/max-area-of-island/solution/by-yusael-rp6d/)：

```js
var maxAreaOfIsland = function (grid) {
    let max = -1
    for (let i = 0; i < grid.length; i++)
        for (let j = 0; j < grid[0].length; j++)
            max = Math.max(max, dfs(grid, i, j))
    return max
};

const dfs = (grid, cr, cc) => {
    // 越界
    if (cr < 0 || cr >= grid.length || cc < 0 || cc >= grid[0].length) return 0
    // 已访问过, 或者不是岛屿
    if (grid[cr][cc] === -1 || grid[cr][cc] === 0) return 0

    // 标记已经访问
    grid[cr][cc] = -1

    let area = 1
    area += dfs(grid, cr - 1, cc) // 上
    area += dfs(grid, cr, cc + 1) // 右
    area += dfs(grid, cr + 1, cc) // 下
    area += dfs(grid, cr, cc - 1) // 左
    return area
}
```

3. 合并二叉树

```js
/**
 * @param {TreeNode} root1
 * @param {TreeNode} root2
 * @return {TreeNode}
 * dfs 解法
 */
var mergeTrees = function(root1, root2) {
    if(!root1 || !root2) return root1 || root2 
    root1.val += root2.val
    root1.left = mergeTrees(root1.left, root2.left)
    root1.right = mergeTrees(root1.right, root2.right)
    return root1
};

/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root1
 * @param {TreeNode} root2
 * @return {TreeNode}
 * bfs 解法
 */
var mergeTrees = function(root1, root2) {
    if(!root1 || !root2) return root1 || root2 
    let root = mergeSingle(root1, root2)
    let queue1 = [root1], queue2 = [root2], queue = [root]
    while(queue1.length && queue2.length) {
        let node = queue.shift(), node1 = queue1.shift(), node2 = queue2.shift()
        node.left = mergeSingle(node1.left, node2.left)
        node.right = mergeSingle(node1.right, node2.right)
        if(node1.left && node2.left) {
            queue1.push(node1.left)
            queue2.push(node2.left)
            queue.push(node.left)
        }
        if(node1.right && node2.right) {
            queue1.push(node1.right)
            queue2.push(node2.right)
            queue.push(node.right)
        }
    }    
    return root
};

var mergeSingle = (node1, node2) => {
    if(!node1 || !node2) return node1 || node2 
    return new TreeNode(node1.val + node2.val)
} 
```

4. 填充每个节点的下一个右侧节点指针

```js
/**
 * @param {Node} root
 * @return {Node}
 * dfs
 */
var connect = function(root) {
    if(!root) return root
    if(root.left) root.left.next = root.right       // 直系的节点
    if(root.right && root.next) root.right.next = root.next.left      // 旁边的兄弟节点
    connect(root.left)
    connect(root.right)
    return root
};

/**
 * @param {Node} root
 * @return {Node}
 * bfs
 */
var connect = function(root) {
    if(!root) return root 
    const queue = [root, null]
    while(queue.length) {
        let first = queue.shift()
        while(first) {      // 两个指针，一层一层的遍历
            let second = queue.shift()
            first.next = second
            if(first.left) queue.push(first.left)
            if(first.right) queue.push(first.right)
            first = second
        }
        if(queue.length) queue.push(null)        // 一层遍历完了
    }
    return root
};
```

5. 01 矩阵

初始化一个二维数组，要么是循环，要么是 Array.fill: `let arr = new Array(3).fill(0).map(_ => new Array(3).fill(0))

```js
/**
 * @param {number[][]} mat
 * @return {number[][]}
 * 使用动态规划
 */
var updateMatrix = function(mat) {
    let res = new Array(mat.length).fill(Number.MAX_VALUE).map(_ => new Array(mat[0].length).fill(Number.MAX_VALUE))
    for(let i = 0; i < mat.length; ++i) {
        for(let j = 0; j < mat[0].length; ++j) {
            if(!mat[i][j]) res[i][j] = 0
        }
    }
    // 从左上角开始
    for(let i = 0; i < mat.length; ++i) {
        for(let j = 0; j < mat[0].length; ++j) {
            if(i - 1 >= 0) res[i][j] = Math.min(res[i-1][j] + 1, res[i][j])
            if(j - 1 >= 0) res[i][j] = Math.min(res[i][j-1] + 1, res[i][j])
        }
    }
    // 从右下角开始
    for(let i = mat.length - 1; i >= 0; --i) {
        for(let j = mat[0].length - 1; j >= 0; --j) {
            if(i + 1 < mat.length) res[i][j] = Math.min(res[i+1][j] + 1, res[i][j])
            if(j + 1 < mat[0].length) res[i][j] = Math.min(res[i][j+1] + 1, res[i][j])
        }
    }
    return res
};
```

6. 腐烂的橘子

