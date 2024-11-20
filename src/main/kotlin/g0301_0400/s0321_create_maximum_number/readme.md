[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 321\. Create Maximum Number

Hard

You are given two integer arrays `nums1` and `nums2` of lengths `m` and `n` respectively. `nums1` and `nums2` represent the digits of two numbers. You are also given an integer `k`.

Create the maximum number of length `k <= m + n` from digits of the two numbers. The relative order of the digits from the same array must be preserved.

Return an array of the `k` digits representing the answer.

**Example 1:**

**Input:** nums1 = [3,4,6,5], nums2 = [9,1,2,5,8,3], k = 5

**Output:** [9,8,6,5,3]

**Example 2:**

**Input:** nums1 = [6,7], nums2 = [6,0,4], k = 5

**Output:** [6,7,6,0,4]

**Example 3:**

**Input:** nums1 = [3,9], nums2 = [8,9], k = 3

**Output:** [9,8,9]

**Constraints:**

*   `m == nums1.length`
*   `n == nums2.length`
*   `1 <= m, n <= 500`
*   `0 <= nums1[i], nums2[i] <= 9`
*   `1 <= k <= m + n`

## Solution

```kotlin
class Solution {
    fun maxNumber(nums1: IntArray, nums2: IntArray, k: Int): IntArray {
        if (k == 0) {
            return IntArray(0)
        }
        val maxSubNums1 = IntArray(k)
        val maxSubNums2 = IntArray(k)
        var res = IntArray(k)
        // select l elements from nums1
        for (l in 0..Math.min(k, nums1.size)) {
            if (l + nums2.size < k) {
                continue
            }
            // create maximum number for each array
            // nums1: l elements; nums2: k - l elements
            maxSubArray(nums1, maxSubNums1, l)
            maxSubArray(nums2, maxSubNums2, k - l)
            // merge the two maximum numbers
            // if get a larger number than res, update res
            res = merge(maxSubNums1, maxSubNums2, l, k - l, res)
        }
        return res
    }

    private fun maxSubArray(nums: IntArray, maxSub: IntArray, size: Int) {
        if (size == 0) {
            return
        }
        var j = 0
        for (i in nums.indices) {
            while (j > 0 && nums.size - i + j > size && nums[i] > maxSub[j - 1]) {
                j--
            }
            if (j < size) {
                maxSub[j++] = nums[i]
            }
        }
    }

    private fun merge(maxSub1: IntArray, maxSub2: IntArray, size1: Int, size2: Int, res: IntArray): IntArray {
        val merge = IntArray(res.size)
        var i = 0
        var j = 0
        var idx = 0
        var equal = true
        while (i < size1 || j < size2) {
            if (j >= size2) {
                merge[idx] = maxSub1[i++]
            } else if (i >= size1) {
                merge[idx] = maxSub2[j++]
            } else {
                var ii = i
                var jj = j
                while (ii < size1 && jj < size2 && maxSub1[ii] == maxSub2[jj]) {
                    ii++
                    jj++
                }
                if (ii < size1 && jj < size2) {
                    if (maxSub1[ii] > maxSub2[jj]) {
                        merge[idx] = maxSub1[i++]
                    } else {
                        merge[idx] = maxSub2[j++]
                    }
                } else if (jj == size2) {
                    merge[idx] = maxSub1[i++]
                } else {
                    // ii == size1
                    merge[idx] = maxSub2[j++]
                }
            }
            // break if we already know merge must be < res
            if (merge[idx] > res[idx]) {
                equal = false
            } else if (equal && merge[idx] < res[idx]) {
                break
            }
            idx++
        }
        // if get a larger number than res, update res
        val k = res.size
        if (i == size1 && j == size2 && !equal) {
            return merge
        }
        return if (equal && merge[k - 1] > res[k - 1]) {
            merge
        } else {
            res
        }
    }
}
```