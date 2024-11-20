[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1348\. Tweet Counts Per Frequency

Medium

A social media company is trying to monitor activity on their site by analyzing the number of tweets that occur in select periods of time. These periods can be partitioned into smaller **time chunks** based on a certain frequency (every **minute**, **hour**, or **day**).

For example, the period `[10, 10000]` (in **seconds**) would be partitioned into the following **time chunks** with these frequencies:

*   Every **minute** (60-second chunks): `[10,69]`, `[70,129]`, `[130,189]`, `...`, `[9970,10000]`
*   Every **hour** (3600-second chunks): `[10,3609]`, `[3610,7209]`, `[7210,10000]`
*   Every **day** (86400-second chunks): `[10,10000]`

Notice that the last chunk may be shorter than the specified frequency's chunk size and will always end with the end time of the period (`10000` in the above example).

Design and implement an API to help the company with their analysis.

Implement the `TweetCounts` class:

*   `TweetCounts()` Initializes the `TweetCounts` object.
*   `void recordTweet(String tweetName, int time)` Stores the `tweetName` at the recorded `time` (in **seconds**).
*   `List<Integer> getTweetCountsPerFrequency(String freq, String tweetName, int startTime, int endTime)` Returns a list of integers representing the number of tweets with `tweetName` in each **time chunk** for the given period of time `[startTime, endTime]` (in **seconds**) and frequency `freq`.
    *   `freq` is one of `"minute"`, `"hour"`, or `"day"` representing a frequency of every **minute**, **hour**, or **day** respectively.

**Example:**

**Input** ["TweetCounts","recordTweet","recordTweet","recordTweet","getTweetCountsPerFrequency","getTweetCountsPerFrequency","recordTweet","getTweetCountsPerFrequency"]

[[],["tweet3",0],["tweet3",60],["tweet3",10],["minute","tweet3",0,59],["minute","tweet3",0,60],["tweet3",120],["hour","tweet3",0,210]]

**Output:** [null,null,null,null,[2],[2,1],null,[4]]

**Explanation:** 

TweetCounts tweetCounts = new TweetCounts(); 

tweetCounts.recordTweet("tweet3", 0); // New tweet "tweet3" at time 0 

tweetCounts.recordTweet("tweet3", 60); // New tweet "tweet3" at time 60 

tweetCounts.recordTweet("tweet3", 10); // New tweet "tweet3" at time 10 

tweetCounts.getTweetCountsPerFrequency("minute", "tweet3", 0, 59); // return [2]; chunk [0,59] had 2 tweets

tweetCounts.getTweetCountsPerFrequency("minute", "tweet3", 0, 60); // return [2,1]; chunk [0,59] had 2 tweets, chunk [60,60] had 1 tweet 

tweetCounts.recordTweet("tweet3", 120); // New tweet "tweet3" at time 120 

tweetCounts.getTweetCountsPerFrequency("hour", "tweet3", 0, 210); // return [4]; chunk [0,210] had 4 tweets

**Constraints:**

*   <code>0 <= time, startTime, endTime <= 10<sup>9</sup></code>
*   <code>0 <= endTime - startTime <= 10<sup>4</sup></code>
*   There will be at most <code>10<sup>4</sup></code> calls **in total** to `recordTweet` and `getTweetCountsPerFrequency`.

## Solution

```kotlin
class TweetCounts {
    private val store: MutableMap<String, MutableMap<Int, MutableMap<Int, MutableMap<Int, MutableList<Int>>>>>

    init {
        store = HashMap()
    }

    fun recordTweet(tweetName: String, time: Int) {
        val d = time / DAY
        val h = (time - d * DAY) / HOUR
        val m = (time - d * DAY - h * HOUR) / MINUTE
        val dstore = store.computeIfAbsent(tweetName) { _: String? -> HashMap() }
        val hstore = dstore.computeIfAbsent(d) { _: Int? -> HashMap() }
        val mstore = hstore.computeIfAbsent(h) { _: Int? -> HashMap() }
        mstore.computeIfAbsent(m) { _: Int? -> ArrayList() }.add(time)
    }

    fun getTweetCountsPerFrequency(
        freq: String,
        tweetName: String,
        startTime: Int,
        endTime: Int,
    ): List<Int> {
        val sfreq = convFreqToSecond(freq)
        val dstore: Map<Int, MutableMap<Int, MutableMap<Int, MutableList<Int>>>> = store[tweetName]!!
        val chunks = IntArray((endTime - startTime) / sfreq + 1)
        val sd = startTime / DAY
        val ed = endTime / DAY
        for (d in sd..ed) {
            if (!dstore.containsKey(d)) {
                continue
            }
            val sh = if (startTime <= d * DAY) 0 else (startTime - d * DAY) / HOUR
            val eh = if (endTime > (d + 1) * DAY) DAY / HOUR else (endTime - d * DAY) / HOUR + 1
            val hstore: Map<Int, MutableMap<Int, MutableList<Int>>> = dstore[d]!!
            for (h in sh until eh) {
                if (!hstore.containsKey(h)) {
                    continue
                }
                val sm = if (startTime <= d * DAY + h * HOUR) {
                    0
                } else {
                    (startTime - d * DAY - h * HOUR) / MINUTE
                }
                val em = if (endTime > d * DAY + (h + 1) * HOUR) {
                    HOUR / MINUTE
                } else {
                    (endTime - d * DAY - h * HOUR) / MINUTE + 1
                }
                val mstore: Map<Int, MutableList<Int>> = hstore[h]!!
                for (m in sm..em) {
                    if (!mstore.containsKey(m)) {
                        continue
                    }
                    for (rc in mstore[m]!!) {
                        if (startTime <= rc && rc <= endTime) {
                            chunks[(rc - startTime) / sfreq]++
                        }
                    }
                }
            }
        }
        val ans: MutableList<Int> = ArrayList()
        for (chunk in chunks) {
            ans.add(chunk)
        }
        return ans
    }

    private fun convFreqToSecond(freq: String): Int {
        return when (freq) {
            "minute" -> MINUTE
            "hour" -> HOUR
            "day" -> DAY
            else -> 0
        }
    }

    companion object {
        private const val MINUTE = 60
        private const val HOUR = 3600
        private const val DAY = 86400
    }
}
```