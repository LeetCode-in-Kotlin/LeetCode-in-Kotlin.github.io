[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 843\. Guess the Word

Hard

This is an **_interactive problem_**.

You are given an array of **unique** strings `wordlist` where `wordlist[i]` is `6` letters long, and one word in this list is chosen as `secret`.

You may call `Master.guess(word)` to guess a word. The guessed word should have type `string` and must be from the original list with `6` lowercase letters.

This function returns an `integer` type, representing the number of exact matches (value and position) of your guess to the `secret` word. Also, if your guess is not in the given wordlist, it will return `-1` instead.

For each test case, you have exactly `10` guesses to guess the word. At the end of any number of calls, if you have made `10` or fewer calls to `Master.guess` and at least one of these guesses was `secret`, then you pass the test case.

**Example 1:**

**Input:** secret = "acckzz", wordlist = ["acckzz","ccbazz","eiowzz","abcczz"], numguesses = 10

**Output:** You guessed the secret word correctly.

**Explanation:**

    master.guess("aaaaaa") returns -1, because "aaaaaa" is not in wordlist.
    master.guess("acckzz") returns 6, because "acckzz" is secret and has all 6 matches.
    master.guess("ccbazz") returns 3, because "ccbazz" has 3 matches.
    master.guess("eiowzz") returns 2, because "eiowzz" has 2 matches.
    master.guess("abcczz") returns 4, because "abcczz" has 4 matches.
    We made 5 calls to master.guess and one of them was the secret, so we pass the test case. 

**Example 2:**

**Input:** secret = "hamada", wordlist = ["hamada","khaled"], numguesses = 10

**Output:** You guessed the secret word correctly.

**Constraints:**

*   `1 <= wordlist.length <= 100`
*   `wordlist[i].length == 6`
*   `wordlist[i]` consist of lowercase English letters.
*   All the strings of `wordlist` are **unique**.
*   `secret` exists in `wordlist`.
*   `numguesses == 10`

## Solution

```kotlin
/*
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface Master {
 *     fun guess(word: String): Int {}
 * }
 */
class Solution {
    interface Master {
        fun guess(word: String): Int
    }

    private var next = 0

    fun findSecretWord(wordlist: Array<String>, master: Master) {
        val list = wordlist.copyOf()
        list.shuffle()
        val test = BooleanArray(wordlist.size)
        while (true) {
            val num = master.guess(list[next])
            if (num == 6) {
                break
            }
            updateList(list, test, num)
        }
    }

    private fun updateList(list: Array<String>, test: BooleanArray, num: Int) {
        val index = next
        for (i in index + 1 until test.size) {
            if (test[i]) {
                continue
            }
            val samePart = getSame(list[index], list[i])
            if (samePart != num) {
                test[i] = true
            } else if (next == index) {
                next = i
            }
        }
    }

    private fun getSame(word1: String, word2: String): Int {
        var ret = 0
        for (i in 0..5) {
            if (word1[i] == word2[i]) {
                ret++
            }
        }
        return ret
    }
}
```