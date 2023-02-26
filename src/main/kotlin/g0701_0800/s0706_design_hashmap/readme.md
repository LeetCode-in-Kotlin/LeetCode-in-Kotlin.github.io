[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 706\. Design HashMap

Easy

Design a HashMap without using any built-in hash table libraries.

Implement the `MyHashMap` class:

*   `MyHashMap()` initializes the object with an empty map.
*   `void put(int key, int value)` inserts a `(key, value)` pair into the HashMap. If the `key` already exists in the map, update the corresponding `value`.
*   `int get(int key)` returns the `value` to which the specified `key` is mapped, or `-1` if this map contains no mapping for the `key`.
*   `void remove(key)` removes the `key` and its corresponding `value` if the map contains the mapping for the `key`.

**Example 1:**

**Input**

["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]

[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]

**Output:** [null, null, null, 1, -1, null, 1, null, -1]

**Explanation:**

    MyHashMap myHashMap = new MyHashMap(); 
    myHashMap.put(1, 1); // The map is now [[1,1]] 
    myHashMap.put(2, 2); // The map is now [[1,1], [2,2]] 
    myHashMap.get(1); // return 1, The map is now [[1,1], [2,2]] 
    myHashMap.get(3); // return -1 (i.e., not found), The map is now [[1,1], [2,2]] 
    myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value) 
    myHashMap.get(2); // return 1, The map is now [[1,1], [2,1]] 
    myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]] 
    myHashMap.get(2); // return -1 (i.e., not found), The map is now [[1,1]]

**Constraints:**

*   <code>0 <= key, value <= 10<sup>6</sup></code>
*   At most <code>10<sup>4</sup></code> calls will be made to `put`, `get`, and `remove`.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class MyHashMap {
    private var arr: Array<MutableList<Entry>?>? = null

    init {
        arr = arrayOfNulls<MutableList<Entry>?>(1000)
    }

    fun put(key: Int, value: Int) {
        val bucket = key % 1000
        if (arr!![bucket] == null) {
            val list = ArrayList<Entry>()
            val e = Entry()
            e.key = key
            e.value = value
            list.add(e)
            arr!![bucket] = list
        } else {
            val list: MutableList<Entry>? = arr!![bucket]
            val e = Entry()
            e.key = key
            e.value = value
            list!!.remove(e)
            list.add(e)
        }
    }

    fun get(key: Int): Int {
        val bucket = key % 1000
        var ans = -1
        val list: ArrayList<Entry>? = arr!![bucket] as ArrayList<Entry>?
        if (list != null) {
            for (e in list) {
                if (e.key == key) {
                    ans = e.value
                }
            }
        }
        return ans
    }

    fun remove(key: Int) {
        val bucket = key % 1000
        val list: ArrayList<Entry>? = arr!![bucket] as ArrayList<Entry>?
        val e = Entry()
        e.key = key
        list?.remove(e)
    }

    internal class Entry {
        var key = 0
        var value = 0
        override fun hashCode(): Int {
            val prime = 31
            var result = 1
            result = prime * result + key
            return result
        }

        override fun equals(other: Any?): Boolean {
            if (this === other) {
                return true
            }
            if (other == null) {
                return false
            }
            if (javaClass != other.javaClass) {
                return false
            }
            val other = other as Entry
            return key == other.key
        }
    }
}

/*
 * Your MyHashMap object will be instantiated and called as such:
 * var obj = MyHashMap()
 * obj.put(key,value)
 * var param_2 = obj.get(key)
 * obj.remove(key)
 */
```