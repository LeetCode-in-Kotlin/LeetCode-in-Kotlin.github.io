[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1108\. Defanging an IP Address

Easy

Given a valid (IPv4) IP `address`, return a defanged version of that IP address.

A _defanged IP address_ replaces every period `"."` with `"[.]"`.

**Example 1:**

**Input:** address = "1.1.1.1"

**Output:** "1[.]1[.]1[.]1"

**Example 2:**

**Input:** address = "255.100.50.0"

**Output:** "255[.]100[.]50[.]0"

**Constraints:**

*   The given `address` is a valid IPv4 address.

## Solution

```kotlin
class Solution {
    fun defangIPaddr(address: String): String {
        return address.replace(".", "[.]")
    }
}
```