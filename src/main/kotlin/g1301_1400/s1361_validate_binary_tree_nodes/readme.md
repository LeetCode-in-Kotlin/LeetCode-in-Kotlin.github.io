[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1361\. Validate Binary Tree Nodes

Medium

You have `n` binary tree nodes numbered from `0` to `n - 1` where node `i` has two children `leftChild[i]` and `rightChild[i]`, return `true` if and only if **all** the given nodes form **exactly one** valid binary tree.

If node `i` has no left child then `leftChild[i]` will equal `-1`, similarly for the right child.

Note that the nodes have no values and that we only use the node numbers in this problem.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/08/23/1503_ex1.png)

**Input:** n = 4, leftChild = [1,-1,3,-1], rightChild = [2,-1,-1,-1]

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/08/23/1503_ex2.png)

**Input:** n = 4, leftChild = [1,-1,3,-1], rightChild = [2,3,-1,-1]

**Output:** false

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/08/23/1503_ex3.png)

**Input:** n = 2, leftChild = [1,0], rightChild = [-1,-1]

**Output:** false

**Constraints:**

*   `n == leftChild.length == rightChild.length`
*   <code>1 <= n <= 10<sup>4</sup></code>
*   `-1 <= leftChild[i], rightChild[i] <= n - 1`

## Solution

```kotlin
import java.util.ArrayDeque
import java.util.Deque

class Solution {
    fun validateBinaryTreeNodes(n: Int, leftChild: IntArray, rightChild: IntArray): Boolean {
        val inDeg = IntArray(n)
        for (i in 0 until n) {
            if (leftChild[i] >= 0) {
                inDeg[leftChild[i]] += 1
            }
            if (rightChild[i] >= 0) {
                inDeg[rightChild[i]] += 1
            }
        }
        val queue: Deque<Int> = ArrayDeque()
        for (i in 0 until n) {
            if (inDeg[i] == 0) {
                if (queue.isEmpty()) {
                    queue.offer(i)
                } else {
                    // Violate rule 1.
                    return false
                }
            }
            if (inDeg[i] > 1) {
                // Violate rule 2.
                return false
            }
        }
        var tpLen = 0
        while (queue.isNotEmpty()) {
            val curNode = queue.poll()
            tpLen++
            val left = leftChild[curNode]
            val right = rightChild[curNode]
            if (left > -1 && --inDeg[left] == 0) {
                queue.offer(left)
            }
            if (right > -1 && --inDeg[right] == 0) {
                queue.offer(right)
            }
        }
        return tpLen == n
    }
}
```