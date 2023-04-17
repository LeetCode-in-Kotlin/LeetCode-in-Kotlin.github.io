[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 912\. Sort an Array

Medium

Given an array of integers `nums`, sort the array in ascending order and return it.

You must solve the problem **without using any built-in** functions in `O(nlog(n))` time complexity and with the smallest space complexity possible.

**Example 1:**

**Input:** nums = [5,2,3,1]

**Output:** [1,2,3,5]

**Explanation:** After sorting the array, the positions of some numbers are not changed (for example, 2 and 3), while the positions of other numbers are changed (for example, 1 and 5).

**Example 2:**

**Input:** nums = [5,1,1,2,0,0]

**Output:** [0,0,1,1,2,5]

**Explanation:** Note that the values of nums are not necessairly unique.

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   <code>-5 * 10<sup>4</sup> <= nums[i] <= 5 * 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun sortArray(nums: IntArray): IntArray {
        return mergeSort(nums, 0, nums.size - 1)
    }

    private fun mergeSort(arr: IntArray, lo: Int, hi: Int): IntArray {
        if (lo == hi) {
            val sortedArr = IntArray(1)
            sortedArr[0] = arr[lo]
            return sortedArr
        }
        val mid = (lo + hi) / 2
        val leftArray = mergeSort(arr, lo, mid)
        val rightArray = mergeSort(arr, mid + 1, hi)
        return mergeSortedArray(leftArray, rightArray)
    }

    private fun mergeSortedArray(a: IntArray, b: IntArray): IntArray {
        val ans = IntArray(a.size + b.size)
        var i = 0
        var j = 0
        var k = 0
        while (i < a.size && j < b.size) {
            if (a[i] < b[j]) {
                ans[k++] = a[i++]
            } else {
                ans[k++] = b[j++]
            }
        }
        while (i < a.size) {
            ans[k++] = a[i++]
        }
        while (j < b.size) {
            ans[k++] = b[j++]
        }
        return ans
    }
}
```