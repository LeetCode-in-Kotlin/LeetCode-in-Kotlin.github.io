[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 332\. Reconstruct Itinerary

Hard

You are given a list of airline `tickets` where <code>tickets[i] = [from<sub>i</sub>, to<sub>i</sub>]</code> represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

*   For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg)

**Input:** tickets = \[\["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]

**Output:** ["JFK","MUC","LHR","SFO","SJC"]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg)

**Input:** tickets = \[\["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]

**Output:** ["JFK","ATL","JFK","SFO","ATL","SFO"]

**Explanation:**

    Another possible reconstruction is
    ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order. 

**Constraints:**

*   `1 <= tickets.length <= 300`
*   `tickets[i].length == 2`
*   <code>from<sub>i</sub>.length == 3</code>
*   <code>to<sub>i</sub>.length == 3</code>
*   <code>from<sub>i</sub></code> and <code>to<sub>i</sub></code> consist of uppercase English letters.
*   <code>from<sub>i</sub> != to<sub>i</sub></code>

## Solution

```kotlin
import java.util.LinkedList
import java.util.PriorityQueue

class Solution {
    fun findItinerary(tickets: List<List<String>>): List<String> {
        val map: HashMap<String, PriorityQueue<String>> = HashMap()
        val ans = LinkedList<String>()
        for (ticket in tickets) {
            val src = ticket[0]
            val dest = ticket[1]
            var pq = map[src]
            if (pq == null) {
                pq = PriorityQueue()
            }
            pq.add(dest)
            map[src] = pq
        }
        dfs(map, "JFK", ans)
        return ans
    }

    private fun dfs(map: Map<String, PriorityQueue<String>>, src: String, ans: LinkedList<String>) {
        val temp = map[src]
        while (!temp.isNullOrEmpty()) {
            val nbr = temp.remove()
            dfs(map, nbr, ans)
        }
        ans.addFirst(src)
    }
}
```