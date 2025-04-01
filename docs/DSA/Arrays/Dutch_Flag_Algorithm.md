# Dutch National Flag Algorithm

## Introduction
The **Dutch National Flag Algorithm** is used to sort an array with three distinct elements efficiently. It is commonly applied to problems involving sorting **0s, 1s, and 2s**.


- Works in **O(N) time** with **O(1) extra space**.
- Uses **three pointers**: `low`, `mid`, and `high`.

---

https://leetcode.com/problems/sort-colors/description/

## Problem Statement
Given an array containing **only 0s, 1s, and 2s**, sort it in-place without using extra space.

### Example:
#### **Input:**  
`[2, 0, 1, 2, 0, 1]`
#### **Output:**  
`[0, 0, 1, 1, 2, 2]`

---

## Algorithm Explanation
1. **Initialize three pointers**:
   - `low` → 0 (Boundary for 0s)
   - `mid` → 0 (Current element being examined)
   - `high` → last index (Boundary for 2s)

2. **Traverse the array** while `mid ≤ high`:
   - If `arr[mid] == 0`:  
     - Swap `arr[low]` and `arr[mid]`
     - Increment `low` and `mid`
   - If `arr[mid] == 1`:  
     - Move `mid` forward
   - If `arr[mid] == 2`:  
     - Swap `arr[mid]` and `arr[high]`
     - Decrement `high`

---

## Code Implementation

### **Java Implementation**
```java
public class DutchFlag {
    public static void sortColors(int[] arr) {
        int low = 0, mid = 0, high = arr.length - 1;

        while (mid <= high) {
            if (arr[mid] == 0) {
                swap(arr, low++, mid++);
            } else if (arr[mid] == 1) {
                mid++;
            } else {
                swap(arr, mid, high--);
            }
        }
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public static void main(String[] args) {
        int[] arr = {2, 0, 2, 1, 1, 0};
        sortColors(arr);
        System.out.println(Arrays.toString(arr)); // Output: [0, 0, 1, 1, 2, 2]
    }
}
