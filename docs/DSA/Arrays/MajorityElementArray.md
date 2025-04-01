-[Majority Problem][https://leetcode.com/problems/majority-element/submissions/1593232264/]


# Boyer-Moore Voting Algorithm

The **Boyer-Moore Voting Algorithm** is an optimal algorithm for finding the majority element in an array. The majority element is defined as the element that appears more than ⌊n / 2⌋ times in the array.

## Key Concepts:
- **Candidate Element**: The element that could potentially be the majority element.
- **Count**: The vote count for the current candidate element.

## Algorithm Overview:
1. Start with the first element as the **candidate** and set the **count** to 1.
2. Traverse through the array, comparing each element with the current **candidate**:
    - If the current element is the same as the candidate, **increase the count**.
    - If the current element is different from the candidate, **decrease the count**.
3. If the **count** reaches zero, change the **candidate** to the current element and reset the **count** to 1.
4. At the end of the traversal, the **candidate** will be the majority element (since it's guaranteed to exist by the problem's constraints).

### Code Implementation:

```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 1;
        int element = nums[0];

        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == element)
                count++;
            else
                count--;

            if (count == 0) {
                element = nums[i];
                count = 1;  // Reset count since we switched the candidate
            }
        }

        return element;
    }
}
