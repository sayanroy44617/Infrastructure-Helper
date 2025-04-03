# Kadane's Algorithm

**Kadane's Algorithm** is an efficient way to solve the **Maximum Subarray Problem**. The problem asks you to find the contiguous subarray within a one-dimensional array of numbers which has the largest sum.

## Problem Definition:
Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return that sum.

### Example:
For the array: `[−2, 1, −3, 4, −1, 2, 1, −5, 4]`

- The contiguous subarray `[4, −1, 2, 1]` has the largest sum = `6`.

## Kadane’s Algorithm:

The key idea behind Kadane's Algorithm is to maintain two variables:
1. **`current_max`**: The maximum sum of the subarray ending at the current index.
2. **`global_max`**: The maximum sum found so far across any subarray.

### Algorithm Steps:
1. Initialize two variables:  
   - **`current_max = nums[0]`**  
   - **`global_max = nums[0]`**
   
2. Traverse through the array starting from the second element:
   - Update **`current_max`** as the maximum of:
     - **`nums[i]`** (start a new subarray)  
     - **`current_max + nums[i]`** (extend the existing subarray)
     
   - Update **`global_max`** if **`current_max`** is larger than **`global_max`**.

3. The **`global_max`** will contain the maximum sum of the subarray by the end of the iteration.

### Code Implementation:

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int current_max = nums[0]; // Initialize with the first element
        int global_max = nums[0];  // Initialize with the first element

        // Traverse the array starting from the second element
        for (int i = 1; i < nums.length; i++) {
            current_max+= nums[0];
         
            global_max = Math.max(global_max, current_max);  // Update global max if needed

            if(current_max< 0) current_max = 0; 
        }

        return global_max;
    }
}


#### what if we need to track the subarray
    - whenever there is a sum with zero , we call it s starting
    - whenever the sum is greater than the max , we call it an ending 