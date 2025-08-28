# 704. Binary Search
```java
class Solution {
    public int search(int[] nums, int target) {
        // Initialize the search window with left and right pointers.
        // 'left' starts at the beginning of the array,
        // 'right' starts at the end of the array.
        int left = 0;
        int right = nums.length - 1;
        
        // Perform binary search as long as the search window is valid.
        while (left <= right) {
            // Calculate the middle index of the current search window.
            // We can also write this as `left + (right - left) / 2` to prevent overflow in large arrays.
            int middle = left + (right - left) / 2;

            // If the element at the middle index is the target, return the index.
            if (target == nums[middle]) {
                return middle;
            }

            // If the target is smaller than the middle element, it must be in the left half.
            // So we shrink the search window by moving 'right' to just before 'middle'.
            else if (target < nums[middle]) {
                right = middle - 1;
            }

            // If the target is larger than the middle element, it must be in the right half.
            // So we shrink the search window by moving 'left' to just after 'middle'.
            else {
                left = middle + 1;
            }
        }

        // If we exit the loop, the target is not in the array. Return -1.
        return -1;
    }
}
```
TC = O(n)  
SC = O(1)
# 27. Remove Element
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        // We are asked to remove all occurrences of `val` in the array `nums`
        // in-place (without allocating extra space), and return the new length
        // of the array after removal. The relative order of elements does not matter.
        
        // i → pointer to scan the array from the beginning
        // j → represents the "effective length" of the array 
        //     (i.e., everything after index j-1 can be ignored/considered removed)
        int i = 0;
        int j = nums.length;
        
        // Loop until i crosses j.
        // i goes forward checking each element.
        // j shrinks backward when we "remove" elements.
        while (i < j) {
            // Case 1: nums[i] is the value we want to remove.
            if (nums[i] == val) {
                // Instead of shifting everything, 
                // we overwrite nums[i] with the last valid element (nums[j-1]).
                nums[i] = nums[j - 1];
                
                // Shrink the "effective array" size by one
                // since we just removed the element at j-1.
                j--;
                
                // Notice: we do NOT increment i here.
                // Why? Because the new element copied from the back (nums[j-1]) 
                // might also equal `val`, so we need to re-check it in the next iteration.
            } else {
                // Case 2: nums[i] is NOT the value to remove.
                // This element is valid, so we move to the next index.
                i++;
            }
        }
        
        // At the end, j points to the new length of the array (all valid elements are before index j).
        return j;
    }
}
```
TC = O(n)  
SC = O(1)
# 977. Squares of a Sorted Array
```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        // We are asked to return a sorted array of the squares of each element in nums.
        // The input array is sorted in non-decreasing order, but it may contain negatives.
        // Negative numbers can produce large squares, so we cannot simply square in order.
        // Example: nums = [-4, -1, 0, 3, 10]
        // Squared = [16, 1, 0, 9, 100] → Sorted = [0, 1, 9, 16, 100].
        //
        // To solve this efficiently in O(n), we use a two-pointer approach.

        int[] result = new int[nums.length]; // Output array of the same size as nums.
        
        // i will track the leftmost element, j will track the rightmost element.
        // Because large squares can come from either extreme (large negative or large positive).
        int i = 0;
        int j = nums.length - 1;
        
        // k will track where to put the next largest square in the result array.
        // We fill from the end (largest square first).
        int k = nums.length - 1;

        // Loop until the two pointers cross
        while (i <= j) {
            // Get absolute values because squaring removes sign,
            // but we care about magnitude when comparing.
            int left = Math.abs(nums[i]);
            int right = Math.abs(nums[j]);

            // Compare absolute values at both ends
            if (left > right) {
                // If left has bigger magnitude, its square is larger.
                result[k] = left * left;
                i++; // Move left pointer forward.
            } else {
                // Otherwise, the right has bigger or equal magnitude.
                result[k] = right * right;
                j--; // Move right pointer backward.
            }

            // After placing the largest square at result[k], move k one step left.
            k--;
        }

        // At the end, result contains all squares in sorted order.
        return result;
    }
}
```
TC = O(n)  
SC = O(n)
