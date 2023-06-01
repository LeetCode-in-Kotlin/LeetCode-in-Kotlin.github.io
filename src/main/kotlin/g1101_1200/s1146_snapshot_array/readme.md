[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1146\. Snapshot Array

Medium

Implement a SnapshotArray that supports the following interface:

*   `SnapshotArray(int length)` initializes an array-like data structure with the given length. **Initially, each element equals 0**.
*   `void set(index, val)` sets the element at the given `index` to be equal to `val`.
*   `int snap()` takes a snapshot of the array and returns the `snap_id`: the total number of times we called `snap()` minus `1`.
*   `int get(index, snap_id)` returns the value at the given `index`, at the time we took the snapshot with the given `snap_id`

**Example 1:**

**Input:** ["SnapshotArray","set","snap","set","get"] [[3],[0,5],[],[0,6],[0,0]]

**Output:** [null,null,0,null,5]

**Explanation:**  

SnapshotArray snapshotArr = new SnapshotArray(3); // set the length to be 3 
snapshotArr.set(0,5); // Set array[0] = 5 
snapshotArr.snap(); // Take a snapshot, return snap_id = 0 
snapshotArr.set(0,6); 
snapshotArr.get(0,0); // Get the value of array[0] with snap_id = 0, return 5

**Constraints:**

*   `1 <= length <= 50000`
*   At most `50000` calls will be made to `set`, `snap`, and `get`.
*   `0 <= index < length`
*   `0 <= snap_id < `(the total number of times we call `snap()`)
*   `0 <= val <= 10^9`

## Solution

```kotlin
import java.util.TreeMap

class SnapshotArray(length: Int) {
    private var snapId = -1
    private val indexToSnapMap: MutableMap<Int, TreeMap<Int, Int>>
    private val ar: IntArray

    init {
        indexToSnapMap = HashMap()
        ar = IntArray(length)
    }

    operator fun set(index: Int, `val`: Int) {
        if (indexToSnapMap.containsKey(index)) {
            if (!indexToSnapMap[index]!!.containsKey(snapId)) {
                indexToSnapMap[index]!![snapId] = ar[index]
            }
        } else {
            val snapToValueMap = TreeMap<Int, Int>()
            snapToValueMap[snapId] = ar[index]
            indexToSnapMap[index] = snapToValueMap
        }
        ar[index] = `val`
    }

    fun snap(): Int {
        snapId++
        return snapId
    }

    operator fun get(index: Int, snapId: Int): Int {
        if (indexToSnapMap.containsKey(index)) {
            val value = indexToSnapMap[index]!!.ceilingEntry(snapId)
            if (value != null) {
                return value.value
            }
        }
        return ar[index]
    }
}
```