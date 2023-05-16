[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 970\. Powerful Integers

Medium

Given three integers `x`, `y`, and `bound`, return _a list of all the **powerful integers** that have a value less than or equal to_ `bound`.

An integer is **powerful** if it can be represented as <code>x<sup>i</sup> + y<sup>j</sup></code> for some integers `i >= 0` and `j >= 0`.

You may return the answer in **any order**. In your answer, each value should occur **at most once**.

**Example 1:**

**Input:** x = 2, y = 3, bound = 10

**Output:** [2,3,4,5,7,9,10]

**Explanation:** 

2 = 2<sup>0</sup> + 3<sup>0</sup> 

3 = 2<sup>1</sup> + 3<sup>0</sup> 

4 = 2<sup>0</sup> + 3<sup>1</sup>

5 = 2<sup>1</sup> + 3<sup>1</sup>

7 = 2<sup>2</sup> + 3<sup>1</sup> 

9 = 2<sup>3</sup> + 3<sup>0</sup> 

10 = 2<sup>0</sup> + 3<sup>2</sup>

**Example 2:**

**Input:** x = 3, y = 5, bound = 15

**Output:** [2,4,6,8,10,14]

**Constraints:**

*   `1 <= x, y <= 100`
*   <code>0 <= bound <= 10<sup>6</sup></code>

## Solution

```kotlin
import kotlin.math.log10
import kotlin.math.pow

class Solution {
    fun powerfulIntegers(x: Int, y: Int, bound: Int): List<Int> {
        val iBound = if (x == 1) 1 else (log10(bound.toDouble()) / log10(x.toDouble())).toInt()
        val jBound = if (y == 1) 1 else (log10(bound.toDouble()) / log10(y.toDouble())).toInt()
        val set: HashSet<Int> = HashSet()
        for (i in 0..iBound) {
            for (j in 0..jBound) {
                val number = (x.toDouble().pow(i.toDouble()) + y.toDouble().pow(j.toDouble())).toInt()
                if (number <= bound) {
                    set.add(number)
                }
            }
        }
        return ArrayList(set)
    }
}
```