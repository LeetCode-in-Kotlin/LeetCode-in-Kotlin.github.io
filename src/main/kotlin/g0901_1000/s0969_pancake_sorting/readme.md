[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 969\. Pancake Sorting

Medium

Given an array of integers `arr`, sort the array by performing a series of **pancake flips**.

In one pancake flip we do the following steps:

*   Choose an integer `k` where `1 <= k <= arr.length`.
*   Reverse the sub-array `arr[0...k-1]` (**0-indexed**).

For example, if `arr = [3,2,1,4]` and we performed a pancake flip choosing `k = 3`, we reverse the sub-array `[3,2,1]`, so <code>arr = [<ins>1</ins>,<ins>2</ins>,<ins>3</ins>,4]</code> after the pancake flip at `k = 3`.

Return _an array of the_ `k`_\-values corresponding to a sequence of pancake flips that sort_ `arr`. Any valid answer that sorts the array within `10 * arr.length` flips will be judged as correct.

**Example 1:**

**Input:** arr = [3,2,4,1]

**Output:** [4,2,4,3]

**Explanation:**  

We perform 4 pancake flips, with k values 4, 2, 4, and 3. 

Starting state: arr = [3, 2, 4, 1] 

After 1st flip (k = 4): arr = [<ins>1</ins>, <ins>4</ins>, <ins>2</ins>, <ins>3</ins>] 

After 2nd flip (k = 2): arr = [<ins>4</ins>, <ins>1</ins>, 2, 3] 

After 3rd flip (k = 4): arr = [<ins>3</ins>, <ins>2</ins>, <ins>1</ins>, <ins>4</ins>] 

After 4th flip (k = 3): arr = [<ins>1</ins>, <ins>2</ins>, <ins>3</ins>, 4], which is sorted.

**Example 2:**

**Input:** arr = [1,2,3]

**Output:** []

**Explanation:** The input is already sorted, so there is no need to flip anything. Note that other answers, such as [3, 3], would also be accepted.

**Constraints:**

*   `1 <= arr.length <= 100`
*   `1 <= arr[i] <= arr.length`
*   All integers in `arr` are unique (i.e. `arr` is a permutation of the integers from `1` to `arr.length`).

## Solution

```kotlin
class Solution {
    fun pancakeSort(arr: IntArray): List<Int> {
        val result: MutableList<Int> = ArrayList()
        for (i in arr.size downTo 1) {
            var max = Int.MIN_VALUE
            var index = 0
            for (j in 0 until i) {
                if (max < arr[j]) {
                    index = j + 1
                    max = arr[j]
                }
            }
            result.add(index)
            reverse(arr, index - 1)
            result.add(i)
            reverse(arr, i - 1)
        }
        return result
    }

    private fun reverse(arr: IntArray, index: Int) {
        for (i in 0..(index - 1) / 2) {
            val temp = arr[i]
            arr[i] = arr[index - i]
            arr[index - i] = temp
        }
    }
}
```