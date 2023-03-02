[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 721\. Accounts Merge

Medium

Given a list of `accounts` where each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are **emails** representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in **any order**.

**Example 1:**

**Input:** accounts =

    [
        ["John","johnsmith@mail.com","john\_newyork@mail.com"],
        ["John","johnsmith@mail.com","john00@mail.com"],
        ["Mary","mary@mail.com"],
        ["John","johnnybravo@mail.com"]
    ]

**Output:**

    [
        ["John","john00@mail.com","john\_newyork@mail.com","johnsmith@mail.com"],
        ["Mary","mary@mail.com"],
        ["John","johnnybravo@mail.com"]
    ]

**Explanation:** The first and second John's are the same person as they have the common email "johnsmith@mail.com". The third John and Mary are different people as none of their email addresses are used by other accounts. We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], ['John', 'john00@mail.com', 'john\_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.

**Example 2:**

**Input:** accounts =

    [
        ["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],
        ["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],
        ["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],
        ["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],
        ["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]
    ]

**Output:**

    [
        ["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],
        ["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],
        ["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],
        ["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],
        ["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]
    ]

**Constraints:**

*   `1 <= accounts.length <= 1000`
*   `2 <= accounts[i].length <= 10`
*   `1 <= accounts[i][j] <= 30`
*   `accounts[i][0]` consists of English letters.
*   `accounts[i][j] (for j > 0)` is a valid email.

## Solution

```kotlin
import java.util.TreeSet

class Solution {
    class UnionFind(size: Int) {
        private var size: Int
        private var numComponents: Int
        private var rank: IntArray
        private var parent: IntArray
        init {
            require(size >= 0) { "Size <= 0 is not allowed" }
            this.size = size
            this.numComponents = size
            rank = IntArray(size) { 1 }
            parent = IntArray(size) { ind -> ind }
        }
        fun find(vertex: Int): Int {
            var root = parent[vertex]
            while (root != parent[root]) {
                parent[root] = parent[parent[root]]
                root = parent[root]
            }
            return root
        }
        fun union(vertex1: Int, vertex2: Int) {
            val firstRoot = find(vertex1)
            val secondRoot = find(vertex2)
            if (firstRoot == secondRoot) {
                return
            }
            if (rank[firstRoot] > rank[secondRoot]) {
                parent[secondRoot] = firstRoot
                rank[firstRoot] += rank[secondRoot]
            } else {
                parent[firstRoot] = secondRoot
                rank[secondRoot] += rank[firstRoot]
            }
            this.numComponents--
        }
        fun size(): Int {
            return size
        }
    }

    fun accountsMerge(accounts: List<List<String>>): List<List<String>> {
        val ownersMap = mutableMapOf<String, Int>()
        val unionFind = UnionFind(accounts.size)
        for (i in accounts.indices) {
            for (j in 1 until accounts[i].size) {
                val mail = accounts[i][j]
                if (!ownersMap.contains(mail)) {
                    ownersMap[mail] = i
                } else {
                    val previousAccount = ownersMap[mail]!!
                    unionFind.union(previousAccount, i)
                }
            }
        }
        val groupsMap = mutableMapOf<Int, TreeSet<String>>()
        for (ind in accounts.indices) {
            val parent = unionFind.find(ind)
            groupsMap.putIfAbsent(parent, TreeSet<String>())
            groupsMap[parent]!!.addAll(accounts[ind].subList(1, accounts[ind].size))
        }
        val res = mutableListOf<List<String>>()
        groupsMap.forEach { (key, value) ->
            val list = mutableListOf<String>()
            list.add(accounts[key][0])
            for (mail in value) {
                list.add(mail)
            }
            res.add(list)
        }
        return res
    }
}
```