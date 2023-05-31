[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1053\. Previous Permutation With One Swap

Medium

Given an array of positive integers `arr` (not necessarily distinct), return _the_ _lexicographically_ _largest permutation that is smaller than_ `arr`, that can be **made with exactly one swap**. If it cannot be done, then return the same array.

**Note** that a _swap_ exchanges the positions of two numbers `arr[i]` and `arr[j]`

**Example 1:**

**Input:** arr = [3,2,1]

**Output:** [3,1,2]

**Explanation:** Swapping 2 and 1.

**Example 2:**

**Input:** arr = [1,1,5]

**Output:** [1,1,5]

**Explanation:** This is already the smallest permutation.

**Example 3:**

**Input:** arr = [1,9,4,6,7]

**Output:** [1,7,4,6,9]

**Explanation:** Swapping 9 and 7.

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>4</sup></code>
*   <code>1 <= arr[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun prevPermOpt1(arr: IntArray): IntArray {
        for (i in arr.indices.reversed()) {
            var diff = Int.MAX_VALUE
            var index = i
            for (j in i + 1 until arr.size) {
                if (arr[i] - arr[j] in 1 until diff) {
                    diff = arr[i] - arr[j]
                    index = j
                }
            }
            if (diff != Int.MAX_VALUE) {
                val temp = arr[i]
                arr[i] = arr[index]
                arr[index] = temp
                break
            }
        }
        return arr
    }
}
```