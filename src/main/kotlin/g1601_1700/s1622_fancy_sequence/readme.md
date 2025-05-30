[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1622\. Fancy Sequence

Hard

Write an API that generates fancy sequences using the `append`, `addAll`, and `multAll` operations.

Implement the `Fancy` class:

*   `Fancy()` Initializes the object with an empty sequence.
*   `void append(val)` Appends an integer `val` to the end of the sequence.
*   `void addAll(inc)` Increments all existing values in the sequence by an integer `inc`.
*   `void multAll(m)` Multiplies all existing values in the sequence by an integer `m`.
*   `int getIndex(idx)` Gets the current value at index `idx` (0-indexed) of the sequence **modulo** <code>10<sup>9</sup> + 7</code>. If the index is greater or equal than the length of the sequence, return `-1`.

**Example 1:**

**Input** ["Fancy", "append", "addAll", "append", "multAll", "getIndex", "addAll", "append", "multAll", "getIndex", "getIndex", "getIndex"] [[], [2], [3], [7], [2], [0], [3], [10], [2], [0], [1], [2]]

**Output:** [null, null, null, null, null, 10, null, null, null, 26, 34, 20]

**Explanation:** 

Fancy fancy = new Fancy(); 

fancy.append(2); // fancy sequence: [2] 

fancy.addAll(3); // fancy sequence: [2+3] -> [5] 

fancy.append(7); // fancy sequence: [5, 7] 

fancy.multAll(2); // fancy sequence: [5\*2, 7\*2] -> [10, 14] 

fancy.getIndex(0); // return 10 

fancy.addAll(3); // fancy sequence: [10+3, 14+3] -> [13, 17] 

fancy.append(10); // fancy sequence: [13, 17, 10] 

fancy.multAll(2); // fancy sequence: [13\*2, 17\*2, 10\*2] -> [26, 34, 20] 

fancy.getIndex(0); // return 26 

fancy.getIndex(1); // return 34 

fancy.getIndex(2); // return 20

**Constraints:**

*   `1 <= val, inc, m <= 100`
*   <code>0 <= idx <= 10<sup>5</sup></code>
*   At most <code>10<sup>5</sup></code> calls total will be made to `append`, `addAll`, `multAll`, and `getIndex`.

## Solution

```kotlin
class Fancy {
    private var values = IntArray(8)
    private var add: Long = 0
    private var mult: Long = 1
    private var rMult: Long = 1
    private var size = 0
    private var inverses = cache()
    fun append(`val`: Int) {
        val result = (`val` - add + MOD) * rMult % MOD
        if (size >= values.size) {
            values = values.copyOf(size + (size shl 1))
        }
        values[size++] = result.toInt()
    }

    fun addAll(inc: Int) {
        add = (add + inc) % MOD
    }

    fun multAll(m: Int) {
        mult = mult * m % MOD
        add = add * m % MOD
        rMult = rMult * inverses[m] % MOD
    }

    fun getIndex(idx: Int): Int {
        return if (idx >= size) {
            -1
        } else {
            ((mult * values[idx] + add) % MOD).toInt()
        }
    }

    private fun multiplicativeInverse(x: Int): Int {
        var y: Long = 1
        val m = MOD.toLong() - 2
        var p = x.toLong()
        var i = 0
        while (1L shl i < m) {
            if (m shr i and 1L == 1L) {
                y = y * p % MOD
            }
            i++
            p = p * p % MOD
        }
        return y.toInt()
    }

    private fun cache(): IntArray {
        inverses = IntArray(101)
        inverses[0] = 0
        inverses[1] = 1
        for (i in 2 until inverses.size) {
            inverses[i] = multiplicativeInverse(i)
        }
        return inverses
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```