[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2954\. Count the Number of Infection Sequences

Hard

You are given an integer `n` and a **0-indexed** integer array `sick` which is **sorted** in **increasing** order.

There are `n` children standing in a queue with positions `0` to `n - 1` assigned to them. The array `sick` contains the positions of the children who are infected with an infectious disease. An infected child at position `i` can spread the disease to either of its immediate neighboring children at positions `i - 1` and `i + 1` **if** they exist and are currently not infected. **At most one** child who was previously not infected can get infected with the disease in one second.

It can be shown that after a finite number of seconds, all the children in the queue will get infected with the disease. An **infection sequence** is the sequential order of positions in which **all** of the non-infected children get infected with the disease. Return _the total number of possible infection sequences_.

Since the answer may be large, return it modulo <code>10<sup>9</sup> + 7</code>.

**Note** that an infection sequence **does not** contain positions of children who were already infected with the disease in the beginning.

**Example 1:**

**Input:** n = 5, sick = [0,4]

**Output:** 4

**Explanation:** Children at positions 1, 2, and 3 are not infected in the beginning. There are 4 possible infection sequences: 
- The children at positions 1 and 3 can get infected since their positions are adjacent to the infected children 0 and 4. The child at position 1 gets infected first. 

Now, the child at position 2 is adjacent to the child at position 1 who is infected and the child at position 3 is adjacent to the child at position 4 who is infected, hence either of them can get infected. The child at position 2 gets infected. Finally, the child at position 3 gets infected because it is adjacent to children at positions 2 and 4 who are infected. The infection sequence is [1,2,3]. 
- The children at positions 1 and 3 can get infected because their positions are adjacent to the infected children 0 and 4. The child at position 1 gets infected first. 

Now, the child at position 2 is adjacent to the child at position 1 who is infected and the child at position 3 is adjacent to the child at position 4 who is infected, hence either of them can get infected. The child at position 3 gets infected. 

Finally, the child at position 2 gets infected because it is adjacent to children at positions 1 and 3 who are infected. The infection sequence is [1,3,2]. 
- The infection sequence is [3,1,2]. The order of infection of disease in the children can be seen as: [<ins>0</ins>,1,2,3,<ins>4</ins>] => [<ins>0</ins>,1,2,<ins>3</ins>,<ins>4</ins>] => [<ins>0</ins>,<ins>1</ins>,2,<ins>3</ins>,<ins>4</ins>] => [<ins>0</ins>,<ins>1</ins>,<ins>2</ins>,<ins>3</ins>,<ins>4</ins>]. 
- The infection sequence is [3,2,1]. The order of infection of disease in the children can be seen as: [<ins>0</ins>,1,2,3,<ins>4</ins>] => [<ins>0</ins>,1,2,<ins>3</ins>,<ins>4</ins>] => [<ins>0</ins>,1,<ins>2</ins>,<ins>3</ins>,<ins>4</ins>] => [<ins>0</ins>,<ins>1</ins>,<ins>2</ins>,<ins>3</ins>,<ins>4</ins>].

**Example 2:**

**Input:** n = 4, sick = [1]

**Output:** 3

**Explanation:** Children at positions 0, 2, and 3 are not infected in the beginning. There are 3 possible infection sequences: 
- The infection sequence is [0,2,3]. The order of infection of disease in the children can be seen as: [0,<ins>1</ins>,2,3] => [<ins>0</ins>,<ins>1</ins>,2,3] => [<ins>0</ins>,<ins>1</ins>,<ins>2</ins>,3] => [<ins>0</ins>,<ins>1</ins>,<ins>2</ins>,<ins>3</ins>]. 
- The infection sequence is [2,0,3]. The order of infection of disease in the children can be seen as: [0,<ins>1</ins>,2,3] => [0,<ins>1</ins>,<ins>2</ins>,3] => [<ins>0</ins>,<ins>1</ins>,<ins>2</ins>,3] => [<ins>0</ins>,<ins>1</ins>,<ins>2</ins>,<ins>3</ins>]. 
- The infection sequence is [2,3,0]. The order of infection of disease in the children can be seen as: [0,<ins>1</ins>,2,3] => [0,<ins>1</ins>,<ins>2</ins>,3] => [0,<ins>1</ins>,<ins>2</ins>,<ins>3</ins>] => [<ins>0</ins>,<ins>1</ins>,<ins>2</ins>,<ins>3</ins>].

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `1 <= sick.length <= n - 1`
*   `0 <= sick[i] <= n - 1`
*   `sick` is sorted in increasing order.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    private val fact = LongArray(M + 1)
    private val invFact = LongArray(M + 1)
    private var init: Long = 0

    private fun modPow(x: Int, y: Int, mod: Int): Int {
        if (y == 0) {
            return 1
        }
        var p = (modPow(x, y / 2, mod) % mod).toLong()
        p = (p * p) % mod
        return if (y % 2 == 1) (p * x % mod).toInt() else p.toInt()
    }

    private fun binomCoeff(n: Int, k: Int): Long {
        return max(
            1.0,
            (fact[n] * invFact[k] % MOD * invFact[n - k] % MOD).toDouble(),
        ).toLong()
    }

    fun numberOfSequence(n: Int, sick: IntArray): Int {
        if (init == 0L) {
            init = 1
            fact[0] = 1
            for (i in 1..M) {
                fact[i] = fact[i - 1] * i % MOD
            }
            invFact[M] = modPow(fact[M].toInt(), MOD - 2, MOD).toLong()
            for (i in M - 1 downTo 1) {
                invFact[i] = invFact[i + 1] * (i + 1) % MOD
            }
        }
        var res: Long = 1
        for (i in 1 until sick.size) {
            val group = sick[i] - sick[i - 1] - 1
            res = res * modPow(2, max(0.0, (group - 1).toDouble()).toInt(), MOD) % MOD
            res = res * binomCoeff(sick[i] - i, group) % MOD
        }
        return (res * binomCoeff(n - sick.size, n - sick[sick.size - 1] - 1) % MOD).toInt()
    }

    companion object {
        private const val M = 100000
        private const val MOD = 1000000007
    }
}
```