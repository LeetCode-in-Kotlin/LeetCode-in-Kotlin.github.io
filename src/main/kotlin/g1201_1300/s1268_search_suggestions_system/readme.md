[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1268\. Search Suggestions System

Medium

You are given an array of strings `products` and a string `searchWord`.

Design a system that suggests at most three product names from `products` after each character of `searchWord` is typed. Suggested products should have common prefix with `searchWord`. If there are more than three products with a common prefix return the three lexicographically minimums products.

Return _a list of lists of the suggested products after each character of_ `searchWord` _is typed_.

**Example 1:**

**Input:** products = ["mobile","mouse","moneypot","monitor","mousepad"], searchWord = "mouse"

**Output:** 
        
    [ 
        ["mobile","moneypot","monitor"], 
        ["mobile","moneypot","monitor"], 
        ["mouse","mousepad"], 
        ["mouse","mousepad"], 
        ["mouse","mousepad"] 
                                ]

**Explanation:** products sorted lexicographically = ["mobile","moneypot","monitor","mouse","mousepad"] After typing m and mo all products match and we show user ["mobile","moneypot","monitor"] After typing mou, mous and mouse the system suggests ["mouse","mousepad"]

**Example 2:**

**Input:** products = ["havana"], searchWord = "havana"

**Output:** [["havana"],["havana"],["havana"],["havana"],["havana"],["havana"]]

**Example 3:**

**Input:** products = ["bags","baggage","banner","box","cloths"], searchWord = "bags"

**Output:** [["baggage","bags","banner"],["baggage","bags","banner"],["baggage","bags"],["bags"]]

**Constraints:**

*   `1 <= products.length <= 1000`
*   `1 <= products[i].length <= 3000`
*   <code>1 <= sum(products[i].length) <= 2 * 10<sup>4</sup></code>
*   All the strings of `products` are **unique**.
*   `products[i]` consists of lowercase English letters.
*   `1 <= searchWord.length <= 1000`
*   `searchWord` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    private var result: MutableList<List<String>> = ArrayList()

    fun suggestedProducts(products: Array<String>, searchWord: String): List<List<String>> {
        // Sort products array first in lexicographically order
        products.sort()
        // Iterate through each "type" of searchWord by using substring
        for (endIndex in 1..searchWord.length) {
            val subSearchWord = searchWord.substring(0, endIndex)
            // Find result for each "type" and add to result list
            val curResult = findResult(products, subSearchWord)
            result.add(curResult)
        }
        return result
    }

    private fun findResult(sortedProducts: Array<String>, searchWord: String): List<String> {
        val curResult: MutableList<String> = ArrayList()
        // Binary search returns the first index possible search result
        val startIndex = binarySearch(sortedProducts, searchWord)
        // Iterate the following 3 string in products or exit if reach end first
        var i = startIndex
        while (i < startIndex + 3 && i < sortedProducts.size) {
            val cur = sortedProducts[i]
            // Only add to curResult if prefix match, otherwise break and return
            if (isPrefix(searchWord, cur)) {
                curResult.add(cur)
            } else {
                return curResult
            }
            i++
        }
        return curResult
    }

    // Compare char by char to check if searchWord is a prefix of product
    private fun isPrefix(searchWord: String, product: String): Boolean {
        for (i in searchWord.indices) {
            val sw = searchWord[i]
            val pr = product[i]
            return if (sw == pr) {
                continue
            } else {
                false
            }
        }
        return true
    }

    // Binary search to find the first index of possible search result
    // The word at the found index should be the least word that's greater or equal to
    // the target search word lexicographically.
    private fun binarySearch(sortedProducts: Array<String>, searchWord: String): Int {
        var start = 0
        var end = sortedProducts.size - 1
        while (start < end) {
            val mid = (start + end) / 2
            val midString = sortedProducts[mid]
            // If mid word is lexicographically less than target word,
            // continue search on right side
            if (searchWord.compareTo(midString) > 0) {
                start = mid + 1
                continue
            }
            // If found the exact match
            if (midString == searchWord) {
                return mid
            }
            // If mid word is lexicographically greater than target word (possible solution)
            if (searchWord.compareTo(midString) < 0) {
                // If mid is at 0,
                // or word at (mid - 1) is If mid word is lexicographically less than target word less than target word, this means we found the least word that's greater than target
                return if (mid == 0 || searchWord.compareTo(sortedProducts[mid - 1]) > 0) {
                    mid
                } else {
                    end = mid - 1
                    continue
                }
            }
        }
        return start
    }
}
```