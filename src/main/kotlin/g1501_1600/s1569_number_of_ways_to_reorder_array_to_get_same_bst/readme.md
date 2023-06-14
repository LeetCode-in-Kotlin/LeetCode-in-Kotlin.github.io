[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1569\. Number of Ways to Reorder Array to Get Same BST

Hard

Given an array `nums` that represents a permutation of integers from `1` to `n`. We are going to construct a binary search tree (BST) by inserting the elements of `nums` in order into an initially empty BST. Find the number of different ways to reorder `nums` so that the constructed BST is identical to that formed from the original array `nums`.

*   For example, given `nums = [2,1,3]`, we will have 2 as the root, 1 as a left child, and 3 as a right child. The array `[2,3,1]` also yields the same BST but `[3,2,1]` yields a different BST.

Return _the number of ways to reorder_ `nums` _such that the BST formed is identical to the original BST formed from_ `nums`.

Since the answer may be very large, **return it modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/12/bb.png)

**Input:** nums = [2,1,3]

**Output:** 1

**Explanation:** We can reorder nums to be [2,3,1] which will yield the same BST. There are no other ways to reorder nums which will yield the same BST.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/12/ex1.png)

**Input:** nums = [3,4,5,1,2]

**Output:** 5

**Explanation:** The following 5 arrays will yield the same BST: 

    [3,1,2,4,5] 
    [3,1,4,2,5] 
    [3,1,4,5,2] 
    [3,4,1,2,5] 
    [3,4,1,5,2]

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/08/12/ex4.png)

**Input:** nums = [1,2,3]

**Output:** 0

**Explanation:** There are no other orderings of nums that will yield the same BST.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i] <= nums.length`
*   All integers in `nums` are **distinct**.

## Solution

```kotlin
class Solution {
    fun numOfWays(nums: IntArray): Int {
        val mod: Long = 1000000007
        val fact = LongArray(1001)
        fact[0] = 1
        for (i in 1..1000) {
            fact[i] = fact[i - 1] * i % mod
        }
        val root = TreeNode(nums[0])
        for (i in 1 until nums.size) {
            addInTree(nums[i], root)
        }
        return ((calcPerms(root, fact).perm - 1) % mod).toInt()
    }

    class Inverse(var x: Long, var y: Long)
    class TreeInfo(var numOfNodes: Long, var perm: Long)
    class TreeNode(var `val`: Int) {
        var left: TreeNode? = null
        var right: TreeNode? = null
    }

    private fun calcPerms(root: TreeNode?, fact: LongArray): TreeInfo {
        val left: TreeInfo
        val right: TreeInfo
        left = if (root!!.left != null) {
            calcPerms(
                root.left, fact
            )
        } else {
            TreeInfo(0, 1)
        }
        right = if (root.right != null) {
            calcPerms(
                root.right, fact
            )
        } else {
            TreeInfo(0, 1)
        }
        val mod: Long = 1000000007
        val totNodes = left.numOfNodes + right.numOfNodes + 1
        val modDiv = getModDivision(
            fact[totNodes.toInt() - 1],
            fact[left.numOfNodes.toInt()],
            fact[right.numOfNodes.toInt()],
            mod
        )
        val perms = if (totNodes == 1L) 1 else left.perm * right.perm % mod * modDiv % mod
        left.numOfNodes = totNodes
        left.perm = perms
        return left
    }

    private fun getModDivision(a: Long, b1: Long, b2: Long, m: Long): Long {
        val b = b1 * b2
        val inv = getInverse(b, m)
        return inv * a % m
    }

    private fun getInverse(b: Long, m: Long): Long {
        val inv = getInverseExtended(b, m)
        return (inv.x % m + m) % m
    }

    private fun getInverseExtended(a: Long, b: Long): Inverse {
        if (a == 0L) {
            return Inverse(0, 1)
        }
        val inv = getInverseExtended(b % a, a)
        val x1 = inv.y - b / a * inv.x
        val y1 = inv.x
        inv.x = x1
        inv.y = y1
        return inv
    }

    private fun addInTree(x: Int, root: TreeNode?) {
        if (root!!.`val` > x) {
            if (root.left != null) {
                addInTree(x, root.left)
            } else {
                root.left = TreeNode(x)
            }
        }
        if (root.`val` < x) {
            if (root.right != null) {
                addInTree(x, root.right)
            } else {
                root.right = TreeNode(x)
            }
        }
    }
}
```