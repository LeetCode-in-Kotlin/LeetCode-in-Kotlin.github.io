[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 460\. LFU Cache

Hard

Design and implement a data structure for a [Least Frequently Used (LFU)](https://en.wikipedia.org/wiki/Least_frequently_used) cache.

Implement the `LFUCache` class:

*   `LFUCache(int capacity)` Initializes the object with the `capacity` of the data structure.
*   `int get(int key)` Gets the value of the `key` if the `key` exists in the cache. Otherwise, returns `-1`.
*   `void put(int key, int value)` Update the value of the `key` if present, or inserts the `key` if not already present. When the cache reaches its `capacity`, it should invalidate and remove the **least frequently used** key before inserting a new item. For this problem, when there is a **tie** (i.e., two or more keys with the same frequency), the **least recently used** `key` would be invalidated.

To determine the least frequently used key, a **use counter** is maintained for each key in the cache. The key with the smallest **use counter** is the least frequently used key.

When a key is first inserted into the cache, its **use counter** is set to `1` (due to the `put` operation). The **use counter** for a key in the cache is incremented either a `get` or `put` operation is called on it.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

**Input**

    ["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"] 
    [[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]

**Output:** [null, null, null, 1, null, -1, 3, null, -1, 3, 4]

**Explanation:**

    // cnt(x) = the use counter for key x 
   
    // cache=[] will show the last used order for tiebreakers (leftmost element is most recent) 
    
    LFUCache lfu = new LFUCache(2); 
    lfu.put(1, 1); // cache=[1,_], cnt(1)=1
    lfu.put(2, 2);  // cache=[2,1], cnt(2)=1, cnt(1)=1
    lfu.get(1);     // return 1 
                    // cache=[1,2], cnt(2)=1, cnt(1)=2
    lfu.put(3, 3);  // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2. 
                    // cache=[3,1], cnt(3)=1, cnt(1)=2
    lfu.get(2);     // return -1 (not found)
    lfu.get(3);     // return 3
                    // cache=[3,1], cnt(3)=2, cnt(1)=2
    lfu.put(4, 4);  // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1. 
                    // cache=[4,3], cnt(4)=1, cnt(3)=2
    lfu.get(1);     // return -1 (not found)
    lfu.get(3);     // return 3 
                    // cache=[3,4], cnt(4)=1, cnt(3)=3
    lfu.get(4);     // return 4 
                    // cache=[3,4], cnt(4)=2, cnt(3)=3

**Constraints:**

*   <code>0 <= capacity <= 10<sup>4</sup></code>
*   <code>0 <= key <= 10<sup>5</sup></code>
*   <code>0 <= value <= 10<sup>9</sup></code>
*   At most <code>2 * 10<sup>5</sup></code> calls will be made to `get` and `put`.

## Solution

```kotlin
class LFUCache(capacity: Int) {
    private class Node {
        var prev: Node? = null
        var next: Node? = null
        var key = -1
        var `val` = 0
        var freq = 0
    }

    private val endOfBlock: MutableMap<Int, Node?>
    private val map: MutableMap<Int, Node>
    private val capacity: Int
    private val linkedList: Node

    init {
        endOfBlock = HashMap()
        map = HashMap()
        this.capacity = capacity
        linkedList = Node()
    }

    operator fun get(key: Int): Int {
        if (map.containsKey(key)) {
            val newEndNode = map[key]
            val endNode: Node?
            val currEndNode = endOfBlock[newEndNode!!.freq]
            if (currEndNode === newEndNode) {
                findNewEndOfBlock(newEndNode)
                if (currEndNode.next == null || currEndNode.next!!.freq > newEndNode.freq + 1) {
                    newEndNode.freq++
                    endOfBlock[newEndNode.freq] = newEndNode
                    return newEndNode.`val`
                }
            }
            if (newEndNode.next != null) {
                newEndNode.next!!.prev = newEndNode.prev
            }
            newEndNode.prev!!.next = newEndNode.next
            newEndNode.freq++
            endNode = if (currEndNode!!.next == null || currEndNode.next!!.freq > newEndNode.freq) {
                currEndNode
            } else {
                endOfBlock[newEndNode.freq]
            }
            endOfBlock[newEndNode.freq] = newEndNode
            if (endNode!!.next != null) {
                endNode.next!!.prev = newEndNode
            }
            newEndNode.next = endNode.next
            endNode.next = newEndNode
            newEndNode.prev = endNode
            return newEndNode.`val`
        }
        return -1
    }

    fun put(key: Int, value: Int) {
        val endNode: Node?
        val newEndNode: Node
        if (capacity == 0) {
            return
        }
        if (map.containsKey(key)) {
            map[key]!!.`val` = value
            get(key)
        } else {
            if (map.size == capacity) {
                val toDelete = linkedList.next
                map.remove(toDelete!!.key)
                if (toDelete.next != null) {
                    toDelete.next!!.prev = linkedList
                }
                linkedList.next = toDelete.next
                if (endOfBlock[toDelete.freq] === toDelete) {
                    endOfBlock.remove(toDelete.freq)
                }
            }
            newEndNode = Node()
            newEndNode.key = key
            newEndNode.`val` = value
            newEndNode.freq = 1
            map[key] = newEndNode
            endNode = endOfBlock.getOrDefault(1, linkedList)
            endOfBlock[1] = newEndNode
            if (endNode!!.next != null) {
                endNode.next!!.prev = newEndNode
            }
            newEndNode.next = endNode.next
            endNode.next = newEndNode
            newEndNode.prev = endNode
        }
    }

    private fun findNewEndOfBlock(node: Node?) {
        val prev = node!!.prev
        if (prev!!.freq == node.freq) {
            endOfBlock[node.freq] = prev
        } else {
            endOfBlock.remove(node.freq)
        }
    }
}

/*
 * Your LFUCache object will be instantiated and called as such:
 * var obj = LFUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```