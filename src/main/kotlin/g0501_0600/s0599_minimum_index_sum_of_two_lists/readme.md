[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 599\. Minimum Index Sum of Two Lists

Easy

Suppose Andy and Doris want to choose a restaurant for dinner, and they both have a list of favorite restaurants represented by strings.

You need to help them find out their **common interest** with the **least list index sum**. If there is a choice tie between answers, output all of them with no order requirement. You could assume there always exists an answer.

**Example 1:**

**Input:** list1 = ["Shogun","Tapioca Express","Burger King","KFC"], list2 = ["Piatti","The Grill at Torrey Pines","Hungry Hunter Steakhouse","Shogun"]

**Output:** ["Shogun"]

**Explanation:** The only restaurant they both like is "Shogun".

**Example 2:**

**Input:** list1 = ["Shogun","Tapioca Express","Burger King","KFC"], list2 = ["KFC","Shogun","Burger King"]

**Output:** ["Shogun"]

**Explanation:** The restaurant they both like and have the least index sum is "Shogun" with index sum 1 (0+1).

**Constraints:**

*   `1 <= list1.length, list2.length <= 1000`
*   `1 <= list1[i].length, list2[i].length <= 30`
*   `list1[i]` and `list2[i]` consist of spaces `' '` and English letters.
*   All the stings of `list1` are **unique**.
*   All the stings of `list2` are **unique**.

## Solution

```kotlin
class Solution {
    fun findRestaurant(list1: Array<String>, list2: Array<String>): Array<String> {
        var min = 1000000
        val hm: MutableMap<String, Int> = HashMap()
        val result: MutableList<String> = ArrayList()
        fillMap(list1, hm)
        // find min value
        for (i in list2.indices) {
            if (hm.containsKey(list2[i])) {
                val value = hm[list2[i]]!! + i
                // a new min value was found
                if (value < min) {
                    min = value
                    // Clean the arraylist
                    result.clear()
                    // add new min value
                    result.add(list2[i])
                } else if (value == min) {
                    result.add(list2[i])
                }
            }
        }
        return result.toTypedArray()
    }

    fun fillMap(a: Array<String>, hm: MutableMap<String, Int>) {
        for (i in a.indices) {
            hm[a[i]] = i
        }
    }
}
```