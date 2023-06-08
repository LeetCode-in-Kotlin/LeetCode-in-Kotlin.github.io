[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1345\. Jump Game IV

Hard

Given an array of integers `arr`, you are initially positioned at the first index of the array.

In one step you can jump from index `i` to index:

*   `i + 1` where: `i + 1 < arr.length`.
*   `i - 1` where: `i - 1 >= 0`.
*   `j` where: `arr[i] == arr[j]` and `i != j`.

Return _the minimum number of steps_ to reach the **last index** of the array.

Notice that you can not jump outside of the array at any time.

**Example 1:**

**Input:** arr = [100,-23,-23,404,100,23,23,23,3,404]

**Output:** 3

**Explanation:** You need three jumps from index 0 --> 4 --> 3 --> 9. Note that index 9 is the last index of the array.

**Example 2:**

**Input:** arr = [7]

**Output:** 0

**Explanation:** Start index is the last index. You do not need to jump.

**Example 3:**

**Input:** arr = [7,6,9,6,9,6,9,7]

**Output:** 1

**Explanation:** You can jump directly from index 0 to index 7 which is last index of the array.

**Constraints:**

*   <code>1 <= arr.length <= 5 * 10<sup>4</sup></code>
*   <code>-10<sup>8</sup> <= arr[i] <= 10<sup>8</sup></code>

## Solution

```kotlin
import java.util.Deque
import java.util.LinkedList

class Solution {
    fun minJumps(arr: IntArray): Int {
        if (arr.size == 1) {
            return 0
        }
        val len = arr.size
        val myHash = HashMap<Int, MutableList<Int>>()
        var i = 0
        while (i < arr.size) {
            val curList = myHash.getOrDefault(arr[i], ArrayList())
            curList.add(i)
            val tempNum = arr[i]
            val tempIndex = i
            while (i < arr.size && arr[i] == tempNum) {
                i++
            }
            if (i != tempIndex + 1) {
                curList.add(i - 1)
            }
            myHash[tempNum] = curList
        }
        val myQueue: Deque<Int> = LinkedList()
        var step = 0
        myQueue.offerLast(0)
        val visited = BooleanArray(arr.size)
        visited[0] = true
        while (myQueue.isNotEmpty()) {
            val curCount = myQueue.size
            var j = 0
            while (j < curCount) {
                val curIndex = myQueue.pollFirst()
                if (curIndex == len - 1) {
                    return step
                }
                if (curIndex + 1 < len && !visited[curIndex + 1]) {
                    myQueue.offerLast(curIndex + 1)
                    visited[curIndex + 1] = true
                }
                if (curIndex - 1 >= 0 && !visited[curIndex - 1]) {
                    myQueue.offerLast(curIndex - 1)
                    visited[curIndex - 1] = true
                }
                val tempList: List<Int> = myHash.getOrDefault(arr[curIndex], ArrayList())
                for (integer in tempList) {
                    if (!visited[integer]) {
                        myQueue.offerLast(integer)
                        visited[integer] = true
                    }
                }
                myHash.remove(arr[curIndex])
                j++
            }
            step++
        }
        return step
    }
}
```