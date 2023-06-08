[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1338\. Reduce Array Size to The Half

Medium

You are given an integer array `arr`. You can choose a set of integers and remove all the occurrences of these integers in the array.

Return _the minimum size of the set so that **at least** half of the integers of the array are removed_.

**Example 1:**

**Input:** arr = [3,3,3,3,5,5,5,2,2,7]

**Output:** 2

**Explanation:** Choosing {3,7} will make the new array [5,5,5,2,2] which has size 5 (i.e equal to half of the size of the old array). 

Possible sets of size 2 are {3,5},{3,2},{5,2}. 

Choosing set {2,7} is not possible as it will make the new array [3,3,3,3,5,5,5] which has a size greater than half of the size of the old array.

**Example 2:**

**Input:** arr = [7,7,7,7,7,7]

**Output:** 1

**Explanation:** The only possible set you can choose is {7}. This will make the new array empty.

**Constraints:**

*   <code>2 <= arr.length <= 10<sup>5</sup></code>
*   `arr.length` is even.
*   <code>1 <= arr[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun minSetSize(arr: IntArray): Int {
        val map: MutableMap<Int, Int> = HashMap()
        for (num in arr) {
            map[num] = map.getOrDefault(num, 0) + 1
        }
        val freq: MutableList<Int> = ArrayList(map.values)
        freq.sortWith(reverseOrder())
        var i = 0
        var count = 0
        var totalLength = arr.size
        while (totalLength > arr.size / 2) {
            totalLength -= freq[i]
            count++
            i++
        }
        return count
    }
}
```