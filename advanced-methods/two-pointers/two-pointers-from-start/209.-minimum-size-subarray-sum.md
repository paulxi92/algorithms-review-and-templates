# 209. Minimum Size Subarray Sum

## Question

Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a **contiguous subarray** `[numsl, numsl+1, ..., numsr-1, numsr]` of which the sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

**Example 2:**

```
Input: target = 4, nums = [1,4,4]
Output: 1
```

**Example 3:**

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

**Constraints:**

* `1 <= target <= 10^9`
* `1 <= nums.length <= 10^5`
* `1 <= nums[i] <= 10^5`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.

## Solution

#### C++

```cpp
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        // sol1: two pointers (two boundaries)
        // int n = nums.size();
        // int left = 0, right = 0, sum = 0, res = n + 1;
        // while (right < n) {
        //     while (sum < s && right < n) { // move right
        //         sum += nums[right++];
        //     }
        //     while (sum >= s) { // move left
        //         sum -= nums[left++];
        //     }
        //     res = min(res, right - left + 1);
        // }
        // return res == n + 1 ? 0 : res;
        
        // sol2: binary search (lower bound)
        int n = nums.size();
        vector<int> sums(n + 1, 0);
        int res = n + 1;
        for (int i = 1; i < n+1; ++i) {
            sums[i] = sums[i-1] + nums[i-1];
        }
        for (int i = 0; i < n; ++i) {
            auto it = lower_bound(sums.begin() + i, sums.end(), sums[i] + s);
            if (it == sums.end()) {
                break;
            }
            int len = it - sums.begin() - i;
            res = min(res, len);
        }
        return res == n + 1 ? 0 : res;
    }
};
```

#### Java

```java
// sol1: two pointers
// class Solution {
//     public int minSubArrayLen(int target, int[] nums) {
//         int n = nums.length;
//         int left = 0, right = 0;
//         int sum = 0;
//         int res = n + 1;
//         while (right < n) {
//             while (sum < target && right < n) { // move right to increase sum
//                 sum += nums[right++];
//             }
//             while (sum >= target && left <= right) {
//                 sum -= nums[left++];
//             }
//             res = Math.min(res, right - left + 1);
//         }
//         return res == n + 1 ? 0 : res;
//     }
// }

// sol2: binary search (lower bound), use cumulative sum
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int[] sums = new int[n + 1];
        int res = n + 1;
        for (int i = 1; i <= n; ++i) {
            sums[i] = sums[i - 1] + nums[i - 1];
        }
        for (int i = 0; i < n; ++i) {
            int idx = lowerBound(i + 1, n + 1, sums[i] + target, sums);
            if (idx == n + 1) {
                continue;
            }
            res = Math.min(res, idx - i);
        }
        return res == n + 1 ? 0 : res;
    }

    private int lowerBound(int start, int end, int target, int[] nums) {
        int left = start, right = end;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```
