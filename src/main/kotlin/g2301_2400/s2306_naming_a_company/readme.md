[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2306\. Naming a Company

Hard

You are given an array of strings `ideas` that represents a list of names to be used in the process of naming a company. The process of naming a company is as follows:

1.  Choose 2 **distinct** names from `ideas`, call them <code>idea<sub>A</sub></code> and <code>idea<sub>B</sub></code>.
2.  Swap the first letters of <code>idea<sub>A</sub></code> and <code>idea<sub>B</sub></code> with each other.
3.  If **both** of the new names are not found in the original `ideas`, then the name <code>idea<sub>A</sub> idea<sub>B</sub></code> (the **concatenation** of <code>idea<sub>A</sub></code> and <code>idea<sub>B</sub></code>, separated by a space) is a valid company name.
4.  Otherwise, it is not a valid name.

Return _the number of **distinct** valid names for the company_.

**Example 1:**

**Input:** ideas = ["coffee","donuts","time","toffee"]

**Output:** 6

**Explanation:** The following selections are valid:

- ("coffee", "donuts"): The company name created is "doffee conuts".

- ("donuts", "coffee"): The company name created is "conuts doffee".

- ("donuts", "time"): The company name created is "tonuts dime".

- ("donuts", "toffee"): The company name created is "tonuts doffee".

- ("time", "donuts"): The company name created is "dime tonuts".

- ("toffee", "donuts"): The company name created is "doffee tonuts".

Therefore, there are a total of 6 distinct company names.


The following are some examples of invalid selections:

- ("coffee", "time"): The name "toffee" formed after swapping already exists in the original array.

- ("time", "toffee"): Both names are still the same after swapping and exist in the original array.

- ("coffee", "toffee"): Both names formed after swapping already exist in the original array. 

**Example 2:**

**Input:** ideas = ["lack","back"]

**Output:** 0

**Explanation:** There are no valid selections. Therefore, 0 is returned. 

**Constraints:**

*   <code>2 <= ideas.length <= 5 * 10<sup>4</sup></code>
*   `1 <= ideas[i].length <= 10`
*   `ideas[i]` consists of lowercase English letters.
*   All the strings in `ideas` are **unique**.

## Solution

```kotlin
class Solution {
    fun distinctNames(a: Array<String>): Long {
        val m = Array<MutableSet<String>>(26) { mutableSetOf() }
        for (s in a) {
            val i = s[0].code - 97
            m[i].add(s.substring(1))
        }

        var res = 0L
        for (i in m.indices) {
            val b1 = m[i]
            if (b1.isEmpty()) {
                continue
            }
            for (y in i + 1 until m.size) {
                val b2 = m[y]
                if (b2.isEmpty()) {
                    continue
                }
                res += compare(b1, b2)
            }
        }
        return res
    }

    fun compare(b1: Set<String>, b2: Set<String>): Long {
        val set1 = if (b1.size > b2.size) b1 else b2
        val set2 = if (b1.size > b2.size) b2 else b1
        var n1 = set1.size
        var n2 = set2.size
        for (s in set1) {
            if (set2.contains(s)) {
                n1--
                n2--
            }
        }
        return (n1 * n2) * 2L
    }
}
```