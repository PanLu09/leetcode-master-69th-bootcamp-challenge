# 509. Fibonacci Number
``` java
class Solution {
    public int fib(int n) {
        // Base case: 
        // If n is 0 or 1, return n directly because:
        // fib(0) = 0, fib(1) = 1.
        if (n <= 1) {
            return n;
        }

        // Initialize variables to store the two previous Fibonacci numbers.
        // nMinus2 represents fib(i-2)
        // nMinus1 represents fib(i-1)
        int nMinus2 = 0;
        int nMinus1 = 1;

        // Variable 'sum' will store the current Fibonacci number (fib(i))
        int sum = nMinus1 + nMinus2;

        // Iterate from 2 up to n to compute fib(n) iteratively.
        // We use iteration instead of recursion to avoid exponential time complexity.
        for (int i = 2; i <= n; i++) {
            // Compute current Fibonacci number as the sum of the previous two.
            sum = nMinus1 + nMinus2;

            // Update the previous two numbers for the next iteration.
            nMinus2 = nMinus1;
            nMinus1 = sum;
        }

        // After the loop ends, 'sum' contains fib(n).
        return sum;
    }
}
```

# 70. Climbing Stairs
``` java
class Solution {
    public int climbStairs(int n) {
        // Base case: 
        // If there is only 1 or 2 steps, the answer is straightforward.
        // - 1 step → only 1 way (1)
        // - 2 steps → either (1+1) or (2), so 2 ways
        if (n <= 2) {
            return n;
        }

        // For n > 2:
        // We'll use an iterative approach to avoid the O(2^n) recursive explosion.
        // This is effectively computing the n-th Fibonacci number since:
        // f(n) = f(n-1) + f(n-2)

        // nMinus1 stores the number of ways to reach step (i-1)
        int nMinus1 = 2;

        // nMinus2 stores the number of ways to reach step (i-2)
        int nMinus2 = 1;

        // sum will store the number of ways to reach the current step i
        int sum = nMinus1 + nMinus2;

        // Start from step 3 up to step n
        // Each iteration updates the number of ways based on previous two results
        for (int i = 3; i <= n; i++) {
            // Current number of ways = sum of the previous two
            sum = nMinus1 + nMinus2;

            // Slide the window forward:
            // - what was (i-1) becomes (i-2)
            // - current sum becomes the new (i-1)
            nMinus2 = nMinus1;
            nMinus1 = sum;
        }

        // After the loop, sum contains the number of ways to reach the n-th step
        return sum;
    }
}
```

# 746. Min Cost Climbing Stairs
``` java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        // The problem: 
        // Each element in the 'cost' array represents the cost of stepping on that stair.
        // You can start from step 0 or step 1, and at each move you can climb either one or two steps.
        // The goal is to reach the top (beyond the last index) with the minimum total cost.

        // We'll use dynamic programming to solve this efficiently in O(n) time and O(1) space.

        // 'downone' will represent the minimum cost to reach one step below the current step (i - 1).
        // 'downtwo' will represent the minimum cost to reach two steps below the current step (i - 2).
        // We initialize both to 0 because before starting, there's no cost yet.
        int downone = 0;
        int downtwo = 0;

        // We start iterating from i = 2 because the first two "positions" (steps 0 and 1)
        // can be reached directly from the ground without any previous cost calculations.
        // We loop until i == cost.length + 1, representing the "top floor" beyond the last stair.
        for (int i = 2; i < (cost.length + 1); i++) {

            // Save the previous 'downone' temporarily before we overwrite it.
            int temp = downone;

            // The recurrence relation:
            // To reach step i, we can either:
            //   - Come from step i-1 and pay cost[i-1]
            //   - Come from step i-2 and pay cost[i-2]
            //
            // We take the minimum of these two options to minimize cost.
            //
            // Example:
            // downone + cost[i - 1] -> cost if coming from one step below
            // downtwo + cost[i - 2] -> cost if coming from two steps below
            downone = Math.min(downone + cost[i - 1], downtwo + cost[i - 2]);

            // Move our window forward:
            // The old 'downone' becomes 'downtwo' for the next iteration.
            downtwo = temp;
        }

        // After the loop, 'downone' contains the minimum total cost to reach the top.
        return downone;
    }
}

```
