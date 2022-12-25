[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 432\. All O\`one Data Structure

Hard

Design a data structure to store the strings' count with the ability to return the strings with minimum and maximum counts.

Implement the `AllOne` class:

*   `AllOne()` Initializes the object of the data structure.
*   `inc(String key)` Increments the count of the string `key` by `1`. If `key` does not exist in the data structure, insert it with count `1`.
*   `dec(String key)` Decrements the count of the string `key` by `1`. If the count of `key` is `0` after the decrement, remove it from the data structure. It is guaranteed that `key` exists in the data structure before the decrement.
*   `getMaxKey()` Returns one of the keys with the maximal count. If no element exists, return an empty string `""`.
*   `getMinKey()` Returns one of the keys with the minimum count. If no element exists, return an empty string `""`.

**Example 1:**

**Input**

    ["AllOne", "inc", "inc", "getMaxKey", "getMinKey", "inc", "getMaxKey", "getMinKey"]
    [[], ["hello"], ["hello"], [], [], ["leet"], [], []]

**Output:** [null, null, null, "hello", "hello", null, "hello", "leet"]

**Explanation:**

    AllOne allOne = new AllOne();
    allOne.inc("hello");
    allOne.inc("hello");
    allOne.getMaxKey(); // return "hello"
    allOne.getMinKey(); // return "hello"
    allOne.inc("leet");
    allOne.getMaxKey(); // return "hello"
    allOne.getMinKey(); // return "leet" 

**Constraints:**

*   `1 <= key.length <= 10`
*   `key` consists of lowercase English letters.
*   It is guaranteed that for each call to `dec`, `key` is existing in the data structure.
*   At most <code>5 * 10<sup>4</sup></code> calls will be made to `inc`, `dec`, `getMaxKey`, and `getMinKey`.

## Solution

```kotlin
class AllOne {
    // maintain a doubly linked list of Buckets
    private val head: Bucket
    private val tail: Bucket

    // for accessing a specific Bucket among the Bucket list in O(1) time
    private val countBucketMap: MutableMap<Int, Bucket?>

    // keep track of count of keys
    private val keyCountMap: MutableMap<String, Int>

    // each Bucket contains all the keys with the same count
    private class Bucket(var count: Int) {
        var keySet: MutableSet<String>
        var next: Bucket? = null
        var pre: Bucket? = null

        init {
            keySet = HashSet()
        }
    }

    /* Initialize your data structure here. */
    init {
        head = Bucket(Int.MIN_VALUE)
        tail = Bucket(Int.MAX_VALUE)
        head.next = tail
        tail.pre = head
        countBucketMap = HashMap()
        keyCountMap = HashMap()
    }

    /* Inserts a new key <Key> with value 1. Or increments an existing key by 1. */
    fun inc(key: String) {
        if (keyCountMap.containsKey(key)) {
            changeKey(key, 1)
        } else {
            keyCountMap[key] = 1
            if (head.next!!.count != 1) {
                addBucketAfter(Bucket(1), head)
            }
            head.next!!.keySet.add(key)
            countBucketMap[1] = head.next
        }
    }

    /* Decrements an existing key by 1. If Key's value is 1, remove it from the data structure. */
    fun dec(key: String) {
        if (keyCountMap.containsKey(key)) {
            val count = keyCountMap[key]!!
            if (count == 1) {
                keyCountMap.remove(key)
                removeKeyFromBucket(countBucketMap[count], key)
            } else {
                changeKey(key, -1)
            }
        }
    }

    /* Returns one of the keys with maximal value. */
    fun getMaxKey(): String {
        return if (tail.pre === head) "" else tail.pre!!.keySet.iterator().next()
    }

    /* Returns one of the keys with Minimal value. */
    fun getMinKey(): String {
        return if (head.next === tail) "" else head.next!!.keySet.iterator().next()
    }

    // helper function to make change on given key according to offset
    private fun changeKey(key: String, offset: Int) {
        val count = keyCountMap[key]!!
        keyCountMap[key] = count + offset
        val curBucket = countBucketMap[count]
        val newBucket: Bucket?
        if (countBucketMap.containsKey(count + offset)) {
            // target Bucket already exists
            newBucket = countBucketMap[count + offset]
        } else {
            // add new Bucket
            newBucket = Bucket(count + offset)
            countBucketMap[count + offset] = newBucket
            addBucketAfter(newBucket, if (offset == 1) curBucket else curBucket!!.pre)
        }
        newBucket!!.keySet.add(key)
        removeKeyFromBucket(curBucket, key)
    }

    private fun removeKeyFromBucket(bucket: Bucket?, key: String) {
        bucket!!.keySet.remove(key)
        if (bucket.keySet.isEmpty()) {
            removeBucketFromList(bucket)
            countBucketMap.remove(bucket.count)
        }
    }

    private fun removeBucketFromList(bucket: Bucket?) {
        bucket!!.pre!!.next = bucket.next
        bucket.next!!.pre = bucket.pre
        bucket.next = null
        bucket.pre = null
    }

    // add newBucket after preBucket
    private fun addBucketAfter(newBucket: Bucket, preBucket: Bucket?) {
        newBucket.pre = preBucket
        newBucket.next = preBucket!!.next
        preBucket.next!!.pre = newBucket
        preBucket.next = newBucket
    }
}

/*
 * Your AllOne object will be instantiated and called as such:
 * var obj = AllOne()
 * obj.inc(key)
 * obj.dec(key)
 * var param_3 = obj.getMaxKey()
 * var param_4 = obj.getMinKey()
 */
```