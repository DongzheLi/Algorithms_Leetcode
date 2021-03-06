# Find mininum in rotated Array

Suppose an array sorteed in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).

Find the minimum element.

Assume there is no duplicate.


## Solution 1: Brute force

Brute force: just iterate over the array, keep an instance variable as the minimal, and update it whenever see a minimal element.

## Solution 2: Binary search

```java
public int findMin(int[] nums) {
    // Initial pointers
    int left  = 0;
    int right = nums.length - 1;
    // If nums is not rotated, min is first element.
    if (nums[left] < nums[right]) return nums[left];
    // While loop for binary search
    while (right - left > 1) {
        int mid = (left + right) / 2;
        if (nums[left] > nums[mid]) right = mid;
        else                        left  = mid;
    }
    return nums[right];
}
```