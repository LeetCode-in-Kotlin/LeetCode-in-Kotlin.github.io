[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1636\. Sort Array by Increasing Frequency

Easy

Given an array of integers `nums`, sort the array in **increasing** order based on the frequency of the values. If multiple values have the same frequency, sort them in **decreasing** order.

Return the _sorted array_.

**Example 1:**

**Input:** nums = [1,1,2,2,2,3]

**Output:** [3,1,1,2,2,2]

**Explanation:** '3' has a frequency of 1, '1' has a frequency of 2, and '2' has a frequency of 3.

**Example 2:**

**Input:** nums = [2,3,1,3,2]

**Output:** [1,3,3,2,2]

**Explanation:** '2' and '3' both have a frequency of 2, so they are sorted in decreasing order.

**Example 3:**

**Input:** nums = [-1,1,-6,4,5,-6,1,4,1]

**Output:** [5,-1,4,4,-6,-6,1,1,1]

**Constraints:**

*   `1 <= nums.length <= 100`
*   `-100 <= nums[i] <= 100`

## Solution

```kotlin
import java.util.Collections
import java.util.TreeMap

class Solution {
    fun frequencySort(nums: IntArray): IntArray {
        val count: MutableMap<Int, Int> = HashMap()
        for (num in nums) {
            count[num] = count.getOrDefault(num, 0) + 1
        }
        val map = TreeMap<Int, MutableList<Int>>()
        for ((key, freq) in count) {
            map.putIfAbsent(freq, ArrayList())
            val list = map[freq]!!
            list.add(key)
            map[freq] = list
        }
        val result = IntArray(nums.size)
        var i = 0
        for (entry in map.entries) {
            val list = entry.value
            list.sortWith(Collections.reverseOrder<Any>())
            var k = entry.key
            var j = 0
            while (j < list.size) {
                while (k-- > 0) {
                    result[i++] = list[j]
                }
                j++
                k = entry.key
            }
        }
        return result
    }
}
```