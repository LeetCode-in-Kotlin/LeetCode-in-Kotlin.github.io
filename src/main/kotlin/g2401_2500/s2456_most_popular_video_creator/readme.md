[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2456\. Most Popular Video Creator

Medium

You are given two string arrays `creators` and `ids`, and an integer array `views`, all of length `n`. The <code>i<sup>th</sup></code> video on a platform was created by `creator[i]`, has an id of `ids[i]`, and has `views[i]` views.

The **popularity** of a creator is the **sum** of the number of views on **all** of the creator's videos. Find the creator with the **highest** popularity and the id of their **most** viewed video.

*   If multiple creators have the highest popularity, find all of them.
*   If multiple videos have the highest view count for a creator, find the lexicographically **smallest** id.

Return _a 2D array of strings_ `answer` _where_ <code>answer[i] = [creator<sub>i</sub>, id<sub>i</sub>]</code> _means that_ <code>creator<sub>i</sub></code> _has the **highest** popularity and_ <code>id<sub>i</sub></code> _is the id of their most popular video._ The answer can be returned in any order.

**Example 1:**

**Input:** creators = ["alice","bob","alice","chris"], ids = ["one","two","three","four"], views = [5,10,5,4]

**Output:** [["alice","one"],["bob","two"]]

**Explanation:** 

The popularity of alice is 5 + 5 = 10. 

The popularity of bob is 10. 

The popularity of chris is 4. 

alice and bob are the most popular creators. 

For bob, the video with the highest view count is "two". 

For alice, the videos with the highest view count are "one" and "three". 

Since "one" is lexicographically smaller than "three", it is included in the answer.

**Example 2:**

**Input:** creators = ["alice","alice","alice"], ids = ["a","b","c"], views = [1,2,2]

**Output:** [["alice","b"]]

**Explanation:** 

The videos with id "b" and "c" have the highest view count. 

Since "b" is lexicographically smaller than "c", it is included in the answer.

**Constraints:**

*   `n == creators.length == ids.length == views.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `1 <= creators[i].length, ids[i].length <= 5`
*   `creators[i]` and `ids[i]` consist only of lowercase English letters.
*   <code>0 <= views[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun mostPopularCreator(creators: Array<String>, ids: Array<String>, views: IntArray): List<List<String>> {
        val totalViews = HashMap<String, Long>()
        val maxView = HashMap<String, Int>()
        var globalMaxView: Long = 0
        for (i in creators.indices) {
            val currentView = totalViews.getOrDefault(creators[i], 0L) + views[i]
            globalMaxView = Math.max(currentView, globalMaxView)
            totalViews[creators[i]] = currentView
            val lastIndex = maxView.getOrDefault(creators[i], -1)
            if (!maxView.containsKey(creators[i]) || views[lastIndex] < views[i] ||
                views[lastIndex] == views[i] && ids[lastIndex].compareTo(
                    ids[i],
                ) > 0
            ) {
                maxView[creators[i]] = i
            }
        }
        val res: MutableList<List<String>> = ArrayList()
        for ((key, value) in totalViews) {
            if (value == globalMaxView) {
                res.add(listOf(key, ids[maxView[key]!!]))
            }
        }
        return res
    }
}
```