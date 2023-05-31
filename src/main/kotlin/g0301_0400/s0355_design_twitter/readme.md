[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 355\. Design Twitter

Medium

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the `10` most recent tweets in the user's news feed.

Implement the `Twitter` class:

*   `Twitter()` Initializes your twitter object.
*   `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
*   `List<Integer> getNewsFeed(int userId)` Retrieves the `10` most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be **ordered from most recent to least recent**.
*   `void follow(int followerId, int followeeId)` The user with ID `followerId` started following the user with ID `followeeId`.
*   `void unfollow(int followerId, int followeeId)` The user with ID `followerId` started unfollowing the user with ID `followeeId`.

**Example 1:**

**Input**

    ["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
    [[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]

**Output:**

    [null, null, [5], null, null, [6, 5], null, [5]]

**Explanation:**

    Twitter twitter = new Twitter();
    twitter.postTweet(1, 5); // User 1 posts a new tweet (id = 5).
    twitter.getNewsFeed(1); // User 1's news feed should return a list with 1 tweet id -> [5]. return [5]
    twitter.follow(1, 2); // User 1 follows user 2.
    twitter.postTweet(2, 6); // User 2 posts a new tweet (id = 6).
    twitter.getNewsFeed(1); // User 1's news feed should return a list with 2 tweet ids -> [6, 5].
      // Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
    twitter.unfollow(1, 2); // User 1 unfollows user 2.
    twitter.getNewsFeed(1); // User 1's news feed should return a list with 1 tweet id -> [5], since user 1 is no longer following user 2. 

**Constraints:**

*   `1 <= userId, followerId, followeeId <= 500`
*   <code>0 <= tweetId <= 10<sup>4</sup></code>
*   All the tweets have **unique** IDs.
*   At most <code>3 * 10<sup>4</sup></code> calls will be made to `postTweet`, `getNewsFeed`, `follow`, and `unfollow`.

## Solution

```kotlin
class Twitter {
    // userId --> user followers
    private val twitterData: MutableMap<Int, HashSet<Int>>

    // head of linked list that stores all the tweets
    private val head: Tweet

    private class Tweet internal constructor(var userId: Int, var tweetId: Int) {
        var next: Tweet? = null
    }

    init {
        head = Tweet(-1, -1)
        twitterData = HashMap()
    }

    fun postTweet(userId: Int, tweetId: Int) {
        checkNewUser(userId)
        val t = Tweet(userId, tweetId)
        val next = head.next
        head.next = t
        t.next = next
    }

    fun getNewsFeed(userId: Int): List<Int> {
        checkNewUser(userId)
        val res: MutableList<Int> = ArrayList()
        val followers = twitterData.getValue(userId)
        var t = head.next
        while (t != null && res.size < 10) {
            if (followers.contains(t.userId)) {
                res.add(t.tweetId)
            }
            t = t.next
        }
        return res
    }

    fun follow(followerId: Int, followeeId: Int) {
        checkNewUser(followeeId)
        checkNewUser(followerId)
        twitterData.getValue(followerId).add(followeeId)
    }

    fun unfollow(followerId: Int, followeeId: Int) {
        checkNewUser(followeeId)
        // cannot unfollower yourself
        if (followerId == followeeId) {
            return
        }
        checkNewUser(followerId)
        twitterData.getValue(followerId).remove(followeeId)
    }

    fun checkNewUser(userId: Int) {
        if (twitterData.containsKey(userId)) {
            return
        }
        twitterData[userId] = HashSet()
        // follow  yourself
        twitterData.getValue(userId).add(userId)
    }
}

/*
 * Your Twitter object will be instantiated and called as such:
 * var obj = Twitter()
 * obj.postTweet(userId,tweetId)
 * var param_2 = obj.getNewsFeed(userId)
 * obj.follow(followerId,followeeId)
 * obj.unfollow(followerId,followeeId)
 */
```