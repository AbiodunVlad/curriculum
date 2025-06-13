# The Sliding Window Technique

## ⏱️ Projected Time

- **Reading & Understanding:** Approximately 30-45 minutes
- **Completing Activities:** Approximately 1-2 hours (depending on practice and problem-solving speed)

## 📚 Prerequisites

- **Basic Programming Concepts, Arrays, and Strings:** Variables, data types, conditional statements, loops (for, while). How to access elements, iterate, and basic operations
  - [JavaScript 2 - Arrays, Functions](/javascript/javascript-2-array-functions.md)
  - [JavaScript 1 - Variables, Strings, Numbers lesson](/javascript/javascript-1-variables.md)
- **Time and Space Complexity (Big O Notation):** A basic understanding of how to analyze algorithm efficiency.
  - [Runtime Complexity](/runtime-complexity/runtime-complexity.md)
  - [Intro to Algorithms](/algorithms/intro-to-algorithms.md)

## 🎯 Motivation: Why Learn This?

Imagine you're working with a huge dataset, like a list of daily stock prices or a long piece of text. Sometimes, you don't need to process the _entire_ dataset at once, but rather focus on a _contiguous section_ of it – like the average stock price over the last 7 days, or the longest word without repeating characters.

A common, but inefficient, approach might be to re-calculate everything for each new section. This is where the Sliding Window Technique shines! It's a clever way to optimize your code, especially when you need to look at "chunks" of data within a larger sequence. Think of it like scanning a long document, but only focusing on a few words at a time, then smoothly moving your focus to the next set of words without re-reading everything.

This isn't just a fancy term; it's a powerful pattern that helps you write faster, more efficient code, avoiding redundant calculations and making your solutions more elegant.

## 💡 Objectives

By the end of this topic, you should be able to:

1.  **Define** what the Sliding Window Technique is and its core principles.
2.  **Identify** problems where the Sliding Window Technique is an appropriate and efficient solution.
3.  **Differentiate** between fixed-size and dynamic-size sliding windows.
4.  **Implement** a fixed-size sliding window solution to find maximum/minimum sums or averages.
5.  **Implement** a dynamic-size sliding window solution to find longest/shortest subarrays or substrings that meet specific conditions.
6.  **Analyze** the time and space complexity of sliding window algorithms.

## 💡 The Core Idea: An Analogy

Think of a physical window that you can slide along a wall.

You're looking at a long mural (your array/string). You can only see a small part of it through your window at any given time.

1. **Fixed Window:** If your window is always the same size (e.g., you always see 3 feet of the mural), you just slide it along. When a new part of the mural comes into view on the right, an old part disappears on the left.
2. **Dynamic Window:** Sometimes, your window might need to grow or shrink. Maybe you're looking for the _smallest_ section of the mural that contains a specific set of colors. Your window expands until it finds those colors, then it might shrink from the left to find the _minimum_ size that still contains them.

![sliding window gif](https://github.com/user-attachments/assets/8768e0d2-43db-4d97-a0d1-75c1ed2c054f)

image credit: [The Simple Engineer video](https://www.youtube.com/watch?v=MK-NZ4hN7rs)

## 🛠️ How It Works: Under the Hood

The "window" is typically defined by two pointers: a `start` pointer and an `end` pointer.

### 1. Fixed-Size Window

This is the simpler case. Your window always has a constant size, let's call it `k`.

- You initialize your window with the first `k` elements.
- You calculate whatever you need (sum, average, count) for this initial window.
- Then, you "slide" the window one element at a time:
  - The `end` pointer moves one step to the right, bringing a new element into the window.
  - The `start` pointer also moves one step to the right, pushing an old element out of the window.
- Crucially, you update your calculation by **subtracting the outgoing element** and **adding the incoming element**, instead of recalculating everything from scratch.

**Visual Example:**

```
Array: [2, 1, 5, 1, 3, 2], k = 3

Step 1: Window [2, 1, 5] → sum = 8
Step 2: Window [1, 5, 1] → sum = 8 - 2 + 1 = 7
Step 3: Window [5, 1, 3] → sum = 7 - 1 + 3 = 9
Step 4: Window [1, 3, 2] → sum = 9 - 5 + 2 = 6
```

### 2. Dynamic-Size Window

This is where it gets a bit more flexible. The window size changes based on certain conditions.

- You typically start with an empty or very small window (`start` and `end` pointers close together).
- You expand the window by moving the `end` pointer to the right, adding new elements.
- As you expand, you check if a certain condition is met (e.g., "does this window contain all required characters?").
- If the condition is met (or violated), you might then try to shrink the window by moving the `start` pointer to the right, removing elements, to find the optimal window size that still satisfies your condition.

**Visual Example:**

```
Finding longest substring without repeating characters in "abcabcbb"

start=0, end=0: "a" → valid, expand
start=0, end=1: "ab" → valid, expand
start=0, end=2: "abc" → valid, expand
start=0, end=3: "abca" → invalid (duplicate 'a'), shrink from left
start=1, end=3: "bca" → valid, continue...
```

## 🤔 When to Use It: Spotting the Clues

Look for these hints in problem statements:

- "Find the **maximum/minimum sum/average** of a **subarray/substring of size K**." (Fixed window)
- "Find the **longest/shortest substring/subarray** that satisfies a certain condition." (Dynamic window)
- "Count the number of subarrays/substrings that meet a criteria."
- Problems involving contiguous sequences where you need to maintain a "state" or "calculation" over a moving range.

<details><summary>
   Evaluate this example and determine how applying this technique enhances codebase:
</summary>
    
```javascript
// Web Analytics Dashboard: Calculate rolling average of daily page views over the last 7 days

class AnalyticsDashboard {
constructor() {
// Sample data: daily page views for the last 30 days
this.dailyPageViews = [
1200, 1350, 980, 1100, 1450, 1600, 1320, // Week 1
1180, 1290, 1050, 1380, 1520, 1700, 1400, // Week 2
1250, 1100, 950, 1200, 1480, 1650, 1380, // Week 3
1150, 1320, 1080, 1420, 1580, 1750, 1450, // Week 4
1300, 1200, 1100 // Last 3 days
];
}

// Naive Approach (inefficient)
calculateRollingAverageNaive(windowSize = 7) {
const rollingAverages = [];

    for (let i = 0; i <= this.dailyPageViews.length - windowSize; i++) {
      let sum = 0;
      // Recalculate sum for each window - O(n*k) time complexity
      for (let j = i; j < i + windowSize; j++) {
        sum += this.dailyPageViews[j];
      }
      rollingAverages.push(Math.round(sum / windowSize));
    }

    return rollingAverages;

}

// Sliding Window Approach (efficient)
calculateRollingAverageOptimized(windowSize = 7) {
if (this.dailyPageViews.length < windowSize) return [];

    const rollingAverages = [];

    // Step 1: Calculate sum of first window
    let windowSum = 0;
    for (let i = 0; i < windowSize; i++) {
      windowSum += this.dailyPageViews[i];
    }
    rollingAverages.push(Math.round(windowSum / windowSize));

    // Step 2: Slide the window - O(n) time complexity
    for (let i = windowSize; i < this.dailyPageViews.length; i++) {
      // Remove the element going out of window (left side)
      windowSum -= this.dailyPageViews[i - windowSize];
      // Add the new element coming into window (right side)
      windowSum += this.dailyPageViews[i];

      rollingAverages.push(Math.round(windowSum / windowSize));
    }

    return rollingAverages;

}

// Real-world usage: Generate dashboard metrics
generateWeeklyReport() {
const sevenDayAverages = this.calculateRollingAverageOptimized(7);
const currentWeekAvg = sevenDayAverages[sevenDayAverages.length - 1];
const previousWeekAvg = sevenDayAverages[sevenDayAverages.length - 2];

    const percentChange = ((currentWeekAvg - previousWeekAvg) / previousWeekAvg * 100).toFixed(1);

    return {
      currentWeekAverage: currentWeekAvg,
      previousWeekAverage: previousWeekAvg,
      percentChange: percentChange,
      trend: percentChange > 0 ? '📈 Up' : '📉 Down',
      allWeeklyAverages: sevenDayAverages
    };

}
}

// Usage Example
const dashboard = new AnalyticsDashboard();

console.log("=== WEB ANALYTICS DASHBOARD ===");
console.log("Daily page views (last 30 days):", dashboard.dailyPageViews);

// Compare performance
console.time("Naive Approach");
const naiveResult = dashboard.calculateRollingAverageNaive(7);
console.timeEnd("Naive Approach");

console.time("Sliding Window Approach");
const optimizedResult = dashboard.calculateRollingAverageOptimized(7);
console.timeEnd("Sliding Window Approach");

console.log("\n7-day rolling averages:", optimizedResult);

// Generate weekly report
const report = dashboard.generateWeeklyReport();
console.log("\n=== WEEKLY REPORT ===");
console.log(`Current week average: ${report.currentWeekAverage} page views`);
console.log(`Previous week average: ${report.previousWeekAverage} page views`);
console.log(`Change: ${report.percentChange}% ${report.trend}`);

// Alert system
if (Math.abs(parseFloat(report.percentChange)) > 10) {
console.log(`🚨 ALERT: Significant change detected (${report.percentChange}%)`);
console.log("Consider investigating traffic sources or recent changes.");
}

```
</details>

## 🚶 Step-by-Step Walkthrough: Max Sum Subarray of Size K

Let's walk through a classic example: "Given an array `[2, 1, 5, 1, 3, 2]` and `k = 3`, find the maximum sum of a subarray of size `k`."

### Step 1: Initialize the Window
- **Array:** `[2, 1, 5, 1, 3, 2]`
- **Window:** First `k=3` elements `[2, 1, 5]`
- **Calculation:** `current_sum = 2 + 1 + 5 = 8`
- **Result:** `max_sum = 8`

### Step 2: Slide the Window (First Move)
- **Window shifts:** `[2, 1, 5]` → `[1, 5, 1]`
- **Outgoing element:** `2` (left side)
- **Incoming element:** `1` (right side)
- **Efficient calculation:** `current_sum = 8 - 2 + 1 = 7`
- **Comparison:** `current_sum` (7) vs `max_sum` (8) → `max_sum` remains `8`

### Step 3: Slide the Window (Second Move)
- **Window shifts:** `[1, 5, 1]` → `[5, 1, 3]`
- **Outgoing element:** `1`
- **Incoming element:** `3`
- **Efficient calculation:** `current_sum = 7 - 1 + 3 = 9`
- **Comparison:** `current_sum` (9) vs `max_sum` (8) → `max_sum` updates to `9`

### Step 4: Continue Sliding
- **Window shifts:** `[5, 1, 3]` → `[1, 3, 2]`
- **Outgoing element:** `5`
- **Incoming element:** `2`
- **Efficient calculation:** `current_sum = 9 - 5 + 2 = 6`
- **Comparison:** `current_sum` (6) vs `max_sum` (9) → `max_sum` remains `9`

### Step 5: End of Array
- **Final Result:** The maximum sum found is `9`

**Key Insight:** Notice how we only did 4 addition/subtraction operations instead of recalculating the sum from scratch each time (which would have been 12 operations total)!

## 📚 Specific Things to Learn

To truly master the Sliding Window Technique, focus on these key aspects:

1.  **Pointer Management:** Understanding how to move `start` and `end` pointers.
    *   For fixed windows, both move together.
    *   For dynamic windows, `end` usually moves first to expand, and `start` moves to shrink.
2.  **Window State:** What information do you need to keep track of *inside* your current window? (e.g., current sum, character frequencies, counts of unique elements).
3.  **Condition Checking:** For dynamic windows, clearly define the condition that determines when to expand the window and when to shrink it.
4.  **Efficient Updates:** The most crucial part! How do you update your window's state (sum, count, etc.) when an element enters and an element leaves, without re-processing the entire window? This is usually an O(1) operation.
5.  **Edge Cases:** What happens if the array is empty, or `k` is larger than the array?
6.  **Problem Recognition:** Practice identifying problems where this technique is applicable. Look for keywords like "contiguous," "subarray," "substring," and "longest/shortest/max/min within a range."

## 📝 Activities

Here are some activities to help you solidify your understanding and practice the Sliding Window Technique:

1.  **Code Along:** Follow the "Max Sum Subarray of Size K" walkthrough in your preferred programming language. Implement the logic step-by-step.

2.  **Fixed Window Practice:**
    *   **Problem:** Given an array of integers and an integer `k`, find the average of all contiguous subarrays of size `k`.
    *   **Hint:** Similar to sum, but divide by `k`.
    *   **Example:** Array `[1, 3, 2, 6, -1, 4, 1, 8, 2]`, k=5 → Output: `[2.2, 2.8, 2.4, 3.6, 2.8]`

3.  **Dynamic Window Practice (Basic):**
    *   **Problem:** Given a string, find the length of the longest substring without repeating characters.
    *   **Hint:** Use a hash map (or dictionary) to keep track of characters seen in the current window and their last index. When a repeat is found, shrink the window from the left.
    *   **Example:** "abcabcbb" → Output: 3 (substring "abc")

4.  **Dynamic Window Practice (Intermediate):**
    *   **Problem:** Given a string `s` and a string `t`, find the minimum window substring of `s` which contains all characters from `t`. If there is no such substring, return the empty string `""`.
    *   **Hint:** Use two hash maps: one for character counts in `t`, and one for character counts in the current window. Keep track of how many characters from `t` are "matched" in the window.
    *   **Example:** s = "ADOBECODEBANC", t = "ABC" → Output: "BANC"

5.  **Explain It:** Try to explain the Sliding Window Technique to a friend or rubber duck. Can you articulate the core idea, how it works, and when to use it in your own words?

## ✨ Conclusion

The Sliding Window Technique is a fundamental pattern in algorithm design, especially for array and string manipulation. It's a fantastic tool to have in your problem-solving toolkit, allowing you to write more efficient and elegant code. Keep practicing, and you'll be sliding through problems like a pro in no time!
```
