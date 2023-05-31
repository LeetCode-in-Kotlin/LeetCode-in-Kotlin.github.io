[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1157\. Online Majority Element In Subarray

Hard

Design a data structure that efficiently finds the **majority element** of a given subarray.

The **majority element** of a subarray is an element that occurs `threshold` times or more in the subarray.

Implementing the `MajorityChecker` class:

*   `MajorityChecker(int[] arr)` Initializes the instance of the class with the given array `arr`.
*   `int query(int left, int right, int threshold)` returns the element in the subarray `arr[left...right]` that occurs at least `threshold` times, or `-1` if no such element exists.

**Example 1:**

**Input** ["MajorityChecker", "query", "query", "query"] [[[1, 1, 2, 2, 1, 1]], [0, 5, 4], [0, 3, 3], [2, 3, 2]]

**Output:** [null, 1, -1, 2]

**Explanation:** 

MajorityChecker majorityChecker = new MajorityChecker([1, 1, 2, 2, 1, 1]); 
majorityChecker.query(0, 5, 4); // return 1 
majorityChecker.query(0, 3, 3); // return -1 
majorityChecker.query(2, 3, 2); // return 2

**Constraints:**

*   <code>1 <= arr.length <= 2 * 10<sup>4</sup></code>
*   <code>1 <= arr[i] <= 2 * 10<sup>4</sup></code>
*   `0 <= left <= right < arr.length`
*   `threshold <= right - left + 1`
*   `2 * threshold > right - left + 1`
*   At most <code>10<sup>4</sup></code> calls will be made to `query`.

## Solution

```kotlin
class MajorityChecker(arr: IntArray) {
    private val valToInd: MutableMap<Int, MutableList<Int>>
    private val bitCount: Array<IntArray>

    init {
        valToInd = HashMap()
        bitCount = Array(arr.size + 1) { IntArray(NUM_OF_BITS) }
        for (i in arr.indices) {
            var `val` = arr[i]
            val indList = valToInd.computeIfAbsent(`val`) { _: Int? -> ArrayList() }
            indList.add(i)
            for (j in 0 until NUM_OF_BITS) {
                bitCount[i + 1][j] = bitCount[i][j] + (`val` and 1)
                `val` = `val` shr 1
            }
        }
    }

    fun query(left: Int, right: Int, threshold: Int): Int {
        var candidateVal = 0
        for (i in NUM_OF_BITS - 1 downTo 0) {
            val curBit = if (bitCount[right + 1][i] - bitCount[left][i] >= threshold) 1 else 0
            candidateVal = (candidateVal shl 1) + curBit
        }
        val indList: List<Int>? = valToInd[candidateVal]
        if (indList == null || indList.size < threshold) {
            return -1
        }
        var indOfLeft = indList.binarySearch(left)
        if (indOfLeft < 0) {
            indOfLeft = -indOfLeft - 1
        }
        var indOfRight = indList.binarySearch(right)
        if (indOfRight < 0) {
            indOfRight = -indOfRight - 2
        }
        return if (indOfRight - indOfLeft + 1 >= threshold) {
            candidateVal
        } else {
            -1
        }
    }

    companion object {
        private const val NUM_OF_BITS = 15
    }
}
```