[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3092\. Most Frequent IDs

Medium

The problem involves tracking the frequency of IDs in a collection that changes over time. You have two integer arrays, `nums` and `freq`, of equal length `n`. Each element in `nums` represents an ID, and the corresponding element in `freq` indicates how many times that ID should be added to or removed from the collection at each step.

*   **Addition of IDs:** If `freq[i]` is positive, it means `freq[i]` IDs with the value `nums[i]` are added to the collection at step `i`.
*   **Removal of IDs:** If `freq[i]` is negative, it means `-freq[i]` IDs with the value `nums[i]` are removed from the collection at step `i`.

Return an array `ans` of length `n`, where `ans[i]` represents the **count** of the _most frequent ID_ in the collection after the <code>i<sup>th</sup></code> step. If the collection is empty at any step, `ans[i]` should be 0 for that step.

**Example 1:**

**Input:** nums = [2,3,2,1], freq = [3,2,-3,1]

**Output:** [3,3,2,2]

**Explanation:**

After step 0, we have 3 IDs with the value of 2. So `ans[0] = 3`.   
After step 1, we have 3 IDs with the value of 2 and 2 IDs with the value of 3. So `ans[1] = 3`.   
After step 2, we have 2 IDs with the value of 3. So `ans[2] = 2`.   
After step 3, we have 2 IDs with the value of 3 and 1 ID with the value of 1. So `ans[3] = 2`.

**Example 2:**

**Input:** nums = [5,5,3], freq = [2,-2,1]

**Output:** [2,0,1]

**Explanation:**

After step 0, we have 2 IDs with the value of 5. So `ans[0] = 2`.   
After step 1, there are no IDs. So `ans[1] = 0`.   
After step 2, we have 1 ID with the value of 3. So `ans[2] = 1`.

**Constraints:**

*   <code>1 <= nums.length == freq.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= freq[i] <= 10<sup>5</sup></code>
*   `freq[i] != 0`
*   The input is generated such that the occurrences of an ID will not be negative in any step.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun mostFrequentIDs(nums: IntArray, freq: IntArray): LongArray {
        var max = Int.MIN_VALUE
        val n = nums.size
        for (num in nums) {
            max = max(max, num)
        }
        val bins = LongArray(max + 1)
        var mostFrequentID = 0
        var maxCount: Long = 0
        val ans = LongArray(n)
        for (i in 0 until n) {
            bins[nums[i]] += freq[i].toLong()
            if (freq[i] > 0) {
                if (bins[nums[i]] > maxCount) {
                    maxCount = bins[nums[i]]
                    mostFrequentID = nums[i]
                }
            } else {
                if (nums[i] == mostFrequentID) {
                    maxCount = bins[nums[i]]
                    for (j in 0..max) {
                        if (bins[j] > maxCount) {
                            maxCount = bins[j]
                            mostFrequentID = j
                        }
                    }
                }
            }
            ans[i] = maxCount
        }
        return ans
    }
}
```