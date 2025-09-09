# 209. Minimum Size Subarray Sum
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        // We want the smallest length of a contiguous subarray
        // such that the sum of its elements is at least `target`.
        // If no such subarray exists, return 0.

        // Initialize the minimum length to the maximum integer value
        // (acts as a sentinel so that any valid subarray length will be smaller).
        int minLength = Integer.MAX_VALUE;

        // Left pointer for our sliding window.
        int left = 0;

        // Running sum of the current window.
        int sum = 0;

        // Expand the window by moving the right pointer.
        for (int right = 0; right < nums.length; right++) {
            // Add the current number into the window sum.
            sum += nums[right];

            // While the window sum is large enough to meet or exceed the target,
            // try to shrink the window from the left to minimize its length.
            while (sum >= target) {
                // Update the minimum length found so far.
                minLength = Math.min(minLength, right - left + 1);

                // Remove the element at the left pointer from the window sum.
                sum -= nums[left];

                // Move the left pointer to shrink the window.
                left++;
            }
        }

        // If no valid subarray was found, minLength will still be Integer.MAX_VALUE.
        // In that case, return 0. Otherwise, return the smallest valid length.
        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
}

```

Time complexity is O(n) because each element is visited at most twice (once by right, once by left).

Space complexity is O(1) because we only use a few integer variables.

# 59. Spiral Matrix II
```java
class Solution {
    public int[][] generateMatrix(int n) {
        // Initialize the result matrix of size n x n
        int[][] matrix = new int[n][n];
        
        // Start filling numbers from 1 up to n^2
        int currNum = 1;

        // We fill the matrix layer by layer.
        // Think of the matrix as made of concentric "rings" or "layers".
        // The outermost ring is layer 0, the next one is layer 1, etc.
        // In total, there are (n+1)/2 layers (integer division handles both odd and even n).
        for (int layer = 0; layer < (n + 1) / 2; layer++) {
            
            // 1. Fill the top row of the current layer, moving left → right.
            // Row index = 'layer', column index varies.
            for (int col = layer; col < (n - layer); col++) {
                matrix[layer][col] = currNum++;
            }

            // 2. Fill the right column of the current layer, moving top → bottom.
            // Column index = (n - layer - 1), row index varies.
            for (int row = layer + 1; row < (n - layer); row++) {
                matrix[row][n - layer - 1] = currNum++;
            }

            // 3. Fill the bottom row of the current layer, moving right → left.
            // Row index = (n - layer - 1), column index decreases.
            // We only do this if we are not overlapping with the top row.
            for (int col = n - layer - 2; col >= layer; col--) {
                matrix[n - layer - 1][col] = currNum++;
            }

            // 4. Fill the left column of the current layer, moving bottom → top.
            // Column index = layer, row index decreases.
            // We only do this if we are not overlapping with the right column.
            for (int row = n - layer - 2; row > layer; row--) {
                matrix[row][layer] = currNum++;
            }
        }

        // After filling all layers, return the matrix.
        return matrix;
    }
}
```
Time Complexity: O(n²) since we fill each cell exactly once.  
Space Complexity:
If we’re counting just the extra space beyond the result, it’s O(1). If we include the result matrix itself, then it’s O(n²) because we must allocate n² cells anyway.”
