# Next Permutation Problem

## 📌 Problem Statement
Given an integer array `nums`, find the **next lexicographically greater permutation** of numbers. If no such permutation exists, rearrange `nums` into the **smallest possible order** (sorted in ascending order).

You must modify the input array **in place** without using extra memory.

---

## 🚀 Approach (Optimal Solution)
The goal is to generate the next permutation **efficiently** in **O(n) time complexity**.

### **🔹 Steps to Solve**
### 1️⃣ Find the Breakpoint
- Traverse the array **from right to left** and find the first index `i` where `nums[i] < nums[i+1]`.
- This **breakpoint** is the position where rearrangement is needed.
- If no such index exists, the array is the **last permutation**, so we reverse it to get the **first permutation**.

### 2️⃣ Find the Right Swap Candidate
- Again, traverse from **right to left** and find the smallest number **larger than `nums[breakpoint]`**.
- Swap this number with `nums[breakpoint]`.

### 3️⃣ Reverse the Suffix
- Reverse the portion of the array after the **breakpoint** (`breakpoint+1` to `end`) to get the smallest lexicographical order.

---

## ✅ **Final Code (Java)**
```java
class Solution {
    public void nextPermutation(int[] nums) {
        int breakpoint = -1;

        if (nums.length == 1) return;

        // Step 1: Find the rightmost breakpoint where nums[i] < nums[i+1]
        for (int i = nums.length - 2; i >= 0; i--) {
            if (nums[i] < nums[i + 1]) {
                breakpoint = i;
                break;
            }
        }

        // If no breakpoint, reverse the entire array (last permutation case)
        if (breakpoint == -1) {
            reverse(nums, 0, nums.length - 1);
            return;
        }

        // Step 2: Find the smallest element larger than nums[breakpoint] to swap
        for (int i = nums.length - 1; i > breakpoint; i--) {
            if (nums[i] > nums[breakpoint]) {
                // Swap and break immediately
                int temp = nums[i];
                nums[i] = nums[breakpoint];
                nums[breakpoint] = temp;
                break;
            }
        }

        // Step 3: Reverse the suffix after the breakpoint to get the next permutation
        reverse(nums, breakpoint + 1, nums.length - 1);
    }

    public void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```

---

## 🛠 **Dry Run Example**
### **Example 1**
#### **Input:**
```java
int[] nums = {1,2,3};
```
#### **Execution:**
- Breakpoint at index `1` (`nums[1] = 2`) since `2 < 3`.
- Swap `2` with `3` → `{1,3,2}`.
- Reverse after index `1` (no change needed).

#### **Output:**
```java
[1,3,2]
```

### **Example 2**
#### **Input:**
```java
int[] nums = {3,2,1};
```
#### **Execution:**
- No breakpoint found (descending order).
- Reverse the array → `{1,2,3}`.

#### **Output:**
```java
[1,2,3]
```

---

## ⏳ **Time Complexity Analysis**
- **Finding the breakpoint:** `O(n)`
- **Finding the rightmost greater element to swap:** `O(n)`
- **Reversing the suffix:** `O(n)`

🔹 **Overall Complexity: O(n) + O(n) + O(n) = O(n)**

---

## ⚡ **Key Takeaways**
- **Understanding Breakpoints** is key to solving this problem.
- **Reversing the suffix** is crucial to ensure the next permutation is lexicographically correct.
- **Edge case handling**: If no breakpoint is found, reverse the entire array.

💡 **This approach efficiently finds the next permutation in O(n) time complexity, making it optimal!** 🚀

