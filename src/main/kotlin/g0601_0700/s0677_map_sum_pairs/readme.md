[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 677\. Map Sum Pairs

Medium

Design a map that allows you to do the following:

*   Maps a string key to a given value.
*   Returns the sum of the values that have a key with a prefix equal to a given string.

Implement the `MapSum` class:

*   `MapSum()` Initializes the `MapSum` object.
*   `void insert(String key, int val)` Inserts the `key-val` pair into the map. If the `key` already existed, the original `key-value` pair will be overridden to the new one.
*   `int sum(string prefix)` Returns the sum of all the pairs' value whose `key` starts with the `prefix`.

**Example 1:**

**Input**

["MapSum", "insert", "sum", "insert", "sum"] [[],

["apple", 3], ["ap"], ["app", 2], ["ap"]]

**Output:** [null, null, 3, null, 5]

**Explanation:**

    MapSum mapSum = new MapSum(); 
    mapSum.insert("apple", 3); 
    mapSum.sum("ap"); // return 3 (apple = 3) 
    mapSum.insert("app", 2); 
    mapSum.sum("ap"); // return 5 (apple + app = 3 + 2 = 5)

**Constraints:**

*   `1 <= key.length, prefix.length <= 50`
*   `key` and `prefix` consist of only lowercase English letters.
*   `1 <= val <= 1000`
*   At most `50` calls will be made to `insert` and `sum`.

## Solution

```kotlin
class MapSum {
    internal class Node {
        var `val`: Int = 0
        var child: Array<Node?> = arrayOfNulls(26)
    }

    private val root: Node = Node()

    fun insert(key: String, `val`: Int) {
        var curr: Node? = root
        for (c in key.toCharArray()) {
            if (curr!!.child[c.code - 'a'.code] == null) {
                curr.child[c.code - 'a'.code] = Node()
            }
            curr = curr.child[c.code - 'a'.code]
        }
        curr!!.`val` = `val`
    }

    private fun sumHelper(root: Node?): Int {
        var o = 0
        for (i in 0..25) {
            if (root!!.child[i] != null) {
                o += root.child[i]!!.`val` + sumHelper(root.child[i])
            }
        }
        return o
    }

    fun sum(prefix: String): Int {
        var curr: Node? = root
        for (c in prefix.toCharArray()) {
            if (curr!!.child[c.code - 'a'.code] == null) {
                return 0
            }
            curr = curr.child[c.code - 'a'.code]
        }
        val sum = curr!!.`val`
        return sum + sumHelper(curr)
    }
}

/*
 * Your MapSum object will be instantiated and called as such:
 * var obj = MapSum()
 * obj.insert(key,`val`)
 * var param_2 = obj.sum(prefix)
 */
```