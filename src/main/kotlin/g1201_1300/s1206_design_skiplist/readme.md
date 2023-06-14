[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1206\. Design Skiplist

Hard

Design a **Skiplist** without using any built-in libraries.

A **skiplist** is a data structure that takes `O(log(n))` time to add, erase and search. Comparing with treap and red-black tree which has the same function and performance, the code length of Skiplist can be comparatively short and the idea behind Skiplists is just simple linked lists.

For example, we have a Skiplist containing `[30,40,50,60,70,90]` and we want to add `80` and `45` into it. The Skiplist works this way:

![](https://assets.leetcode.com/uploads/2019/09/27/1506_skiplist.gif)  
Artyom Kalinin [CC BY-SA 3.0], via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Skip_list_add_element-en.gif "Artyom Kalinin [CC BY-SA 3.0 (https://creativecommons.org/licenses/by-sa/3.0)], via Wikimedia Commons")

You can see there are many layers in the Skiplist. Each layer is a sorted linked list. With the help of the top layers, add, erase and search can be faster than `O(n)`. It can be proven that the average time complexity for each operation is `O(log(n))` and space complexity is `O(n)`.

See more about Skiplist: [https://en.wikipedia.org/wiki/Skip\_list](https://en.wikipedia.org/wiki/Skip_list)

Implement the `Skiplist` class:

*   `Skiplist()` Initializes the object of the skiplist.
*   `bool search(int target)` Returns `true` if the integer `target` exists in the Skiplist or `false` otherwise.
*   `void add(int num)` Inserts the value `num` into the SkipList.
*   `bool erase(int num)` Removes the value `num` from the Skiplist and returns `true`. If `num` does not exist in the Skiplist, do nothing and return `false`. If there exist multiple `num` values, removing any one of them is fine.

Note that duplicates may exist in the Skiplist, your code needs to handle this situation.

**Example 1:**

**Input**

["Skiplist", "add", "add", "add", "search", "add", "search", "erase", "erase", "search"]

[[], [1], [2], [3], [0], [4], [1], [0], [1], [1]]

**Output:** [null, null, null, null, false, null, true, false, true, false]

**Explanation:** 

    Skiplist skiplist = new Skiplist(); 
    skiplist.add(1); 
    skiplist.add(2); 
    skiplist.add(3); 
    skiplist.search(0); // return False 
    skiplist.add(4); 
    skiplist.search(1); // return True 
    skiplist.erase(0); // return False, 0 is not in skiplist. 
    skiplist.erase(1); // return True 
    skiplist.search(1); // return False, 1 has already been erased.

**Constraints:**

*   <code>0 <= num, target <= 2 * 10<sup>4</sup></code>
*   At most <code>5 * 10<sup>4</sup></code> calls will be made to `search`, `add`, and `erase`.

## Solution

```kotlin
@Suppress("NAME_SHADOWING", "kotlin:S2245")
class Skiplist @JvmOverloads constructor(size: Int = INIT_CAPACITY) {
    private val minBoundary: Int
    private val head: Node
    private var headCapacity: Int
    private var headLevel = 0

    class Node internal constructor(val `val`: Int, level: Int) {
        var next: Array<Node?>

        init {
            next = arrayOfNulls(level)
        }
    }

    init {
        var size = size
        require(size != 0) { "size should be greater than 0" }
        if (size < INIT_CAPACITY) {
            size = INIT_CAPACITY
        }
        minBoundary = size / 2
        headCapacity = size
        head = Node(0, size)
    }

    fun search(target: Int): Boolean {
        var curr: Node? = head
        for (i in headLevel - 1 downTo 0) {
            while (curr!!.next[i] != null) {
                val cmp = target - curr.next[i]!!.`val`
                curr = if (cmp < 0) {
                    break
                } else if (cmp > 0) {
                    curr.next[i]
                } else {
                    return true
                }
            }
        }
        return false
    }

    fun add(num: Int) {
        val update = arrayOfNulls<Node>(headLevel + 1)
        update[headLevel] = head
        buildUpdate(num, update)
        val level = randomLevel
        if (level > headLevel) {
            if (headLevel == headCapacity) {
                resizeHead(2 * headCapacity)
            }
            headLevel++
        }
        val x = Node(num, level)
        for (i in 0 until level) {
            val n = update[i]!!.next[i]
            update[i]!!.next[i] = x
            x.next[i] = n
        }
    }

    fun erase(num: Int): Boolean {
        if (headLevel == 0) {
            return false
        }
        val update = arrayOfNulls<Node>(headLevel)
        buildUpdate(num, update)
        if (update[0]!!.next[0] == null || update[0]!!.next[0]!!.`val` != num) {
            return false
        }
        for (i in 0 until headLevel) {
            if (update[i]!!.next[i] == null || update[i]!!.next[i]!!.`val` != num) {
                break
            }
            update[i]!!.next[i] = update[i]!!.next[i]!!.next[i]
        }
        if (head.next[headLevel - 1] == null && --headLevel >= minBoundary && headLevel == headCapacity / 4) {
            resizeHead(headCapacity / 2)
        }
        return true
    }

    private fun buildUpdate(x: Int, update: Array<Node?>) {
        var curr: Node? = head
        for (i in headLevel - 1 downTo 0) {
            while (curr!!.next[i] != null && curr.next[i]!!.`val` < x) {
                curr = curr.next[i]
            }
            update[i] = curr
        }
    }

    private val randomLevel: Int
        get() {
            val maxLevel = 14
            var level = 1
            val limit = maxLevel.coerceAtMost(headLevel + 1)
            val p = 0.5
            while (Math.random() < p && level < limit) {
                level++
            }
            return level
        }

    private fun resizeHead(size: Int) {
        val copy = arrayOfNulls<Node>(size)
        System.arraycopy(head.next, 0, copy, 0, headLevel)
        head.next = copy
        headCapacity = size
    }

    companion object {
        private const val INIT_CAPACITY = 8
    }
}
```