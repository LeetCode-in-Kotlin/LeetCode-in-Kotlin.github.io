[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2353\. Design a Food Rating System

Medium

Design a food rating system that can do the following:

*   **Modify** the rating of a food item listed in the system.
*   Return the highest-rated food item for a type of cuisine in the system.

Implement the `FoodRatings` class:

*   `FoodRatings(String[] foods, String[] cuisines, int[] ratings)` Initializes the system. The food items are described by `foods`, `cuisines` and `ratings`, all of which have a length of `n`.
    *   `foods[i]` is the name of the <code>i<sup>th</sup></code> food,
    *   `cuisines[i]` is the type of cuisine of the <code>i<sup>th</sup></code> food, and
    *   `ratings[i]` is the initial rating of the <code>i<sup>th</sup></code> food.
*   `void changeRating(String food, int newRating)` Changes the rating of the food item with the name `food`.
*   `String highestRated(String cuisine)` Returns the name of the food item that has the highest rating for the given type of `cuisine`. If there is a tie, return the item with the **lexicographically smaller** name.

Note that a string `x` is lexicographically smaller than string `y` if `x` comes before `y` in dictionary order, that is, either `x` is a prefix of `y`, or if `i` is the first position such that `x[i] != y[i]`, then `x[i]` comes before `y[i]` in alphabetic order.

**Example 1:**

**Input**

["FoodRatings", "highestRated", "highestRated", "changeRating", "highestRated", "changeRating", "highestRated"]

[[["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]], ["korean"], ["japanese"], ["sushi", 16], ["japanese"], ["ramen", 16], ["japanese"]]

**Output:**

[null, "kimchi", "ramen", null, "sushi", null, "ramen"]

**Explanation:**

    FoodRatings foodRatings = new FoodRatings(["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]);
    foodRatings.highestRated("korean"); // return "kimchi"
                                        // "kimchi" is the highest rated korean food with a rating of 9.
    foodRatings.highestRated("japanese"); // return "ramen"
                                          // "ramen" is the highest rated japanese food with a rating of 14.
    foodRatings.changeRating("sushi", 16); // "sushi" now has a rating of 16.
    foodRatings.highestRated("japanese"); // return "sushi"
                                          // "sushi" is the highest rated japanese food with a rating of 16.
    foodRatings.changeRating("ramen", 16); // "ramen" now has a rating of 16.
    foodRatings.highestRated("japanese"); // return "ramen"
                                          // Both "sushi" and "ramen" have a rating of 16.
                                          // However, "ramen" is lexicographically smaller than "sushi". 

**Constraints:**

*   <code>1 <= n <= 2 * 10<sup>4</sup></code>
*   `n == foods.length == cuisines.length == ratings.length`
*   `1 <= foods[i].length, cuisines[i].length <= 10`
*   `foods[i]`, `cuisines[i]` consist of lowercase English letters.
*   <code>1 <= ratings[i] <= 10<sup>8</sup></code>
*   All the strings in `foods` are **distinct**.
*   `food` will be the name of a food item in the system across all calls to `changeRating`.
*   `cuisine` will be a type of cuisine of **at least one** food item in the system across all calls to `highestRated`.
*   At most <code>2 * 10<sup>4</sup></code> calls **in total** will be made to `changeRating` and `highestRated`.

## Solution

```kotlin
import java.util.TreeSet

class FoodRatings(foods: Array<String>, cuisines: Array<String>, ratings: IntArray) {
    private val cus = HashMap<String, TreeSet<Food?>>()
    private val foodHashMap = HashMap<String, Food>()

    init {
        for (i in foods.indices) {
            val food = Food(foods[i], ratings[i], cuisines[i])
            foodHashMap[foods[i]] = food
            if (cus.containsKey(cuisines[i])) {
                cus[cuisines[i]]!!.add(food)
            } else {
                val pq = TreeSet(Comp())
                pq.add(food)
                cus[cuisines[i]] = pq
            }
        }
    }

    fun changeRating(food: String, newRating: Int) {
        val dish = foodHashMap[food]
        val pq = cus[dish!!.cus]!!
        pq.remove(dish)
        dish.rating = newRating
        pq.add(dish)
    }

    fun highestRated(cuisine: String): String {
        return cus[cuisine]!!.first()!!.foodItem
    }

    private class Comp : Comparator<Food> {
        override fun compare(f1: Food, f2: Food): Int {
            return if (f1.rating == f2.rating) {
                f1.foodItem.compareTo(f2.foodItem)
            } else {
                Integer.compare(f2.rating, f1.rating)
            }
        }
    }

    private class Food internal constructor(val foodItem: String, var rating: Int, val cus: String)
}
/*
 * Your FoodRatings object will be instantiated and called as such:
 * var obj = FoodRatings(foods, cuisines, ratings)
 * obj.changeRating(food,newRating)
 * var param_2 = obj.highestRated(cuisine)
 */
```