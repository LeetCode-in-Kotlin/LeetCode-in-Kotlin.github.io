[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1311\. Get Watched Videos by Your Friends

Medium

There are `n` people, each person has a unique _id_ between `0` and `n-1`. Given the arrays `watchedVideos` and `friends`, where `watchedVideos[i]` and `friends[i]` contain the list of watched videos and the list of friends respectively for the person with `id = i`.

Level **1** of videos are all watched videos by your friends, level **2** of videos are all watched videos by the friends of your friends and so on. In general, the level `k` of videos are all watched videos by people with the shortest path **exactly** equal to `k` with you. Given your `id` and the `level` of videos, return the list of videos ordered by their frequencies (increasing). For videos with the same frequency order them alphabetically from least to greatest.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/01/02/leetcode_friends_1.png)**

**Input:** watchedVideos = \[\["A","B"],["C"],["B","C"],["D"]], friends = \[\[1,2],[0,3],[0,3],[1,2]], id = 0, level = 1

**Output:** ["B","C"]

**Explanation:** 

You have id = 0 (green color in the figure) and your friends are (yellow color in the figure): 

Person with id = 1 -> watchedVideos = ["C"] 

Person with id = 2 -> watchedVideos = ["B","C"] 

The frequencies of watchedVideos by your friends are: 

B -> 1 

C -> 2

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/01/02/leetcode_friends_2.png)**

**Input:** watchedVideos = \[\["A","B"],["C"],["B","C"],["D"]], friends = \[\[1,2],[0,3],[0,3],[1,2]], id = 0, level = 2

**Output:** ["D"]

**Explanation:** You have id = 0 (green color in the figure) and the only friend of your friends is the person with id = 3 (yellow color in the figure).

**Constraints:**

*   `n == watchedVideos.length == friends.length`
*   `2 <= n <= 100`
*   `1 <= watchedVideos[i].length <= 100`
*   `1 <= watchedVideos[i][j].length <= 8`
*   `0 <= friends[i].length < n`
*   `0 <= friends[i][j] < n`
*   `0 <= id < n`
*   `1 <= level < n`
*   if `friends[i]` contains `j`, then `friends[j]` contains `i`

## Solution

```kotlin
import java.util.LinkedList
import java.util.PriorityQueue
import java.util.Queue

class Solution {
    internal class VideoCount(var v: String, var count: Int) {
        override fun toString(): String {
            return "$v $count"
        }
    }

    fun watchedVideosByFriends(
        watchedVideos: List<List<String>>,
        friends: Array<IntArray>,
        id: Int,
        level: Int,
    ): List<String> {
        val visited = BooleanArray(watchedVideos.size)
        val queue: Queue<Int> = LinkedList()
        queue.add(id)
        visited[id] = true
        var currLevel = 0
        while (queue.isNotEmpty()) {
            var size = queue.size
            while (size-- > 0) {
                val node = queue.poll()
                val nei = friends[node]
                for (i in nei) {
                    if (!visited[i]) {
                        queue.add(i)
                        visited[i] = true
                    }
                }
            }
            currLevel++
            if (currLevel == level) {
                break
            }
        }
        val map: MutableMap<String, VideoCount> = HashMap()
        while (queue.isNotEmpty()) {
            val f = queue.poll()
            val watchedVideo = watchedVideos[f]
            for (video in watchedVideo) {
                map.putIfAbsent(video, VideoCount(video, 0))
                map[video]!!.count++
            }
        }
        val pq = PriorityQueue { v1: VideoCount, v2: VideoCount,
            ->
            if (v1.count == v2.count) v1.v.compareTo(v2.v) else v1.count - v2.count
        }
        for ((_, value) in map) {
            pq.add(value)
        }
        val res: MutableList<String> = ArrayList()
        while (pq.isNotEmpty()) {
            res.add(pq.poll().v)
        }
        return res
    }
}
```