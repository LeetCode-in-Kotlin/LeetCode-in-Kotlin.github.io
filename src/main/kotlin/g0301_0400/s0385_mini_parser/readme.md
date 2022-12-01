[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 385\. Mini Parser

Medium

Given a string s represents the serialization of a nested list, implement a parser to deserialize it and return _the deserialized_ `NestedInteger`.

Each element is either an integer or a list whose elements may also be integers or other lists.

**Example 1:**

**Input:** s = "324"

**Output:** 324

**Explanation:** You should return a NestedInteger object which contains a single integer 324.

**Example 2:**

**Input:** s = "[123,[456,[789]]]"

**Output:** [123,[456,[789]]]

**Explanation:**
Return a NestedInteger object containing a nested list with 2 elements:
1. An integer containing value 123.
2. A nested list containing two elements:

   i. An integer containing value 456.

   ii. A nested list with one element:

   a. An integer containing value 789

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>4</sup></code>
*   `s` consists of digits, square brackets `"[]"`, negative sign `'-'`, and commas `','`.
*   `s` is the serialization of valid `NestedInteger`.
*   All the values in the input are in the range <code>[-10<sup>6</sup>, 10<sup>6</sup>]</code>.

## Solution

```kotlin
import com_github_leetcode.NestedInteger

/*
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     constructor()
 *
 *     // Constructor initializes a single integer.
 *     constructor(value: Int)
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     fun isInteger(): Boolean
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     fun getInteger(): Int?
 *
 *     // Set this NestedInteger to hold a single integer.
 *     fun setInteger(value: Int): Unit
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     fun add(ni: NestedInteger): Unit
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     fun getList(): List<NestedInteger>?
 * }
 */
class Solution {
    private var i = 0
    fun deserialize(s: String): NestedInteger {
        return getAns(s)
    }

    private fun getAns(s: String): NestedInteger {
        return if (s[i] == '[') {
            val ni = NestedInteger()
            i++
            while (i < s.length && s[i] != ']') {
                ni.add(getAns(s))
            }
            i++
            ni
        } else if (s[i] == ',') {
            i++
            getAns(s)
        } else {
            var x = 0
            var m = 1
            if (s[i] == '-') {
                i++
                m = -1
            }
            while (i < s.length && Character.isDigit(s[i])) {
                x = x * 10 + s[i++].code - '0'.code
            }
            x *= m
            NestedInteger(x)
        }
    }
}
```