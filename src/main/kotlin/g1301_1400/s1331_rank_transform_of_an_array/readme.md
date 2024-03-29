[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1331\. Rank Transform of an Array

Easy

Given an array of integers `arr`, replace each element with its rank.

The rank represents how large the element is. The rank has the following rules:

*   Rank is an integer starting from 1.
*   The larger the element, the larger the rank. If two elements are equal, their rank must be the same.
*   Rank should be as small as possible.

**Example 1:**

**Input:** arr = [40,10,20,30]

**Output:** [4,1,2,3]

**Explanation:** 40 is the largest element. 10 is the smallest. 20 is the second smallest. 30 is the third smallest.

**Example 2:**

**Input:** arr = [100,100,100]

**Output:** [1,1,1]

**Explanation:** Same elements share the same rank.

**Example 3:**

**Input:** arr = [37,12,28,9,100,56,80,5,12]

**Output:** [5,3,4,2,8,6,7,1,3]

**Constraints:**

*   <code>0 <= arr.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= arr[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun arrayRankTransform(arr: IntArray): IntArray {
        val tmp = arr.copyOf(arr.size)
        tmp.sort()
        val mp = HashMap<Int, Int>()
        var i = 1
        for (x in tmp) {
            if (!mp.containsKey(x)) {
                mp[x] = i++
            }
        }
        i = 0
        for (x in arr) {
            arr[i++] = mp[x]!!
        }
        return arr
    }
}
```