[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 382\. Linked List Random Node

Medium

Given a singly linked list, return a random node's value from the linked list. Each node must have the **same probability** of being chosen.

Implement the `Solution` class:

*   `Solution(ListNode head)` Initializes the object with the integer array nums.
*   `int getRandom()` Chooses a node randomly from the list and returns its value. All the nodes of the list should be equally likely to be choosen.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/16/getrand-linked-list.jpg)

**Input**

    ["Solution", "getRandom", "getRandom", "getRandom", "getRandom", "getRandom"] 
    [[[1, 2, 3]], [], [], [], [], []]

**Output:**

    [null, 1, 3, 2, 2, 3]

**Explanation:**

    Solution solution = new Solution([1, 2, 3]); 
    solution.getRandom(); // return 1 
    solution.getRandom(); // return 3 
    solution.getRandom(); // return 2 
    solution.getRandom(); // return 2 
    solution.getRandom(); // return 3 
    // getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.

**Constraints:**

*   The number of nodes in the linked list will be in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>-10<sup>4</sup> <= Node.val <= 10<sup>4</sup></code>
*   At most <code>10<sup>4</sup></code> calls will be made to `getRandom`.

**Follow up:**

*   What if the linked list is extremely large and its length is unknown to you?
*   Could you solve this efficiently without using extra space?

## Solution

```kotlin
import com_github_leetcode.ListNode
import kotlin.random.Random

/*
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
@Suppress("NAME_SHADOWING", "kotlin:S2245")
class Solution(head: ListNode?) {
    private val al: MutableList<Int>

    init {
        var head = head
        al = ArrayList()
        while (head != null) {
            al.add(head.`val`)
            head = head.next
        }
    }

    fun getRandom(): Int {
        /*
        Math.random() will generate a random number b/w 0 & 1.
        then multiply it with the array size.
        take only the integer part which is a random index.
        return the element at that random index.
         */
        val ind = Random.nextInt(al.size)
        return al[ind]
    }
}

/*
 * Your Solution object will be instantiated and called as such:
 * var obj = Solution(head)
 * var param_1 = obj.getRandom()
 */
```