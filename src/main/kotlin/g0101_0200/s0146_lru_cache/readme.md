[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 146\. LRU Cache

Medium

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

*   `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
*   `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
*   `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

**Input** ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"] [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]

**Output:** [null, null, null, 1, null, -1, null, -1, 3, 4]

**Explanation:** 

LRUCache lRUCache = new LRUCache(2); 

lRUCache.put(1, 1); // cache is {1=1} 

lRUCache.put(2, 2); // cache is {1=1, 2=2} 

lRUCache.get(1); // return 1 

lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3} 

lRUCache.get(2); // returns -1 (not found) 

lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}

lRUCache.get(1); // return -1 (not found) 

lRUCache.get(3); // return 3 

lRUCache.get(4); // return 4

**Constraints:**

*   `1 <= capacity <= 3000`
*   <code>0 <= key <= 10<sup>4</sup></code>
*   <code>0 <= value <= 10<sup>5</sup></code>
*   At most <code>2 * 10<sup>5</sup></code> calls will be made to `get` and `put`.

## Solution

```kotlin
class LRUCache(capacity: Int) {
    private val nodeMap = HashMap<Int, Node>()
    private val head = Node()
    private val tail = Node()
    private var cacheCapacity = 0

    init {
        head.next = tail
        tail.prev = head
        cacheCapacity = capacity
    }

    fun get(key: Int): Int {
        return if (!nodeMap.containsKey(key)) {
            -1
        } else {
            val requiredNode = nodeMap.getValue(key)
            update(requiredNode)
            requiredNode.value
        }
    }

    fun put(key: Int, value: Int) {
        if (nodeMap.containsKey(key)) {
            val requiredNode = nodeMap.getValue(key)
            requiredNode.value = value
            update(requiredNode)
        } else {
            val newNode = Node(key = key, value = value)
            if (nodeMap.size >= cacheCapacity) {
                val nodeToDelete = tail.prev!!
                remove(nodeToDelete)
                nodeMap.remove(nodeToDelete.key)
            }
            add(newNode)
            nodeMap.put(key, newNode)
        }
    }

    fun add(node: Node) {
        val after = head.next
        head.next = node
        node.prev = head
        node.next = after
        after?.prev = node
    }

    private fun remove(node: Node) {
        val before = node.prev
        val after = node.next

        before?.next = after
        after?.prev = before
    }

    private fun update(node: Node) {
        remove(node)
        add(node)
    }

    data class Node(var key: Int = -1, var value: Int = -1, var next: Node? = null, var prev: Node? = null)
}

/*
 * Your LRUCache object will be instantiated and called as such:
 * var obj = LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```