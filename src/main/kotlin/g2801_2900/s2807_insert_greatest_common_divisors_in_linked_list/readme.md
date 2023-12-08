[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2807\. Insert Greatest Common Divisors in Linked List

Medium

Given the head of a linked list `head`, in which each node contains an integer value.

Between every pair of adjacent nodes, insert a new node with a value equal to the **greatest common divisor** of them.

Return _the linked list after insertion_.

The **greatest common divisor** of two numbers is the largest positive integer that evenly divides both numbers.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/07/18/ex1_copy.png)

**Input:** head = [18,6,10,3]

**Output:** [18,6,6,2,10,1,3]

**Explanation:** The 1<sup>st</sup> diagram denotes the initial linked list and the 2<sup>nd</sup> diagram denotes the linked list after inserting the new nodes (nodes in blue are the inserted nodes). 

- We insert the greatest common divisor of 18 and 6 = 6 between the 1<sup>st</sup> and the 2<sup>nd</sup> nodes. 
- We insert the greatest common divisor of 6 and 10 = 2 between the 2<sup>nd</sup> and the 3<sup>rd</sup> nodes. 
- We insert the greatest common divisor of 10 and 3 = 1 between the 3<sup>rd</sup> and the 4<sup>th</sup> nodes. 

There are no more adjacent nodes, so we return the linked list.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/07/18/ex2_copy1.png)

**Input:** head = [7]

**Output:** [7]

**Explanation:** The 1<sup>st</sup> diagram denotes the initial linked list and the 2<sup>nd</sup> diagram denotes the linked list after inserting the new nodes. There are no pairs of adjacent nodes, so we return the initial linked list.

**Constraints:**

*   The number of nodes in the list is in the range `[1, 5000]`.
*   `1 <= Node.val <= 1000`

## Solution

```kotlin
import com_github_leetcode.ListNode

/*
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
class Solution {
    fun insertGreatestCommonDivisors(head: ListNode?): ListNode? {
        var prevNode: ListNode? = null
        var currNode = head
        while (currNode != null) {
            if (prevNode != null) {
                val gcd = greatestCommonDivisor(prevNode.`val`, currNode.`val`)
                prevNode.next = ListNode(gcd, currNode)
            }
            prevNode = currNode
            currNode = currNode.next
        }
        return head
    }

    private fun greatestCommonDivisor(val1: Int, val2: Int): Int {
        return if (val2 == 0) {
            val1
        } else greatestCommonDivisor(val2, val1 % val2)
    }
}
```