# 349. Intersection of Two Arrays

## Question

Given two integer arrays `nums1` and `nums2`, return _an array of their intersection_. Each element in the result must be **unique** and you may return the result in **any order**.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
```

**Constraints:**

* `1 <= nums1.length, nums2.length <= 1000`
* `0 <= nums1[i], nums2[i] <= 1000`

## Solution

#### C++

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        // sol1: hashset
        // unordered_set<int> st (nums1.begin(), nums1.end());
        // unordered_set<int> res;
        // for (auto num : nums2) {
        //     if (st.count(num)) {
        //         res.insert(num);
        //     }
        // }
        // return vector<int>(res.begin(), res.end());
        
        // sol2: sort, two pointers
        // vector<int> res;
        // int i = 0, j = 0;
        // sort(nums1.begin(), nums1.end());
        // sort(nums2.begin(), nums2.end());
        // while (i < nums1.size() && j < nums2.size()) {
        //     if (nums1[i] < nums2[j]) {
        //         ++i;
        //     } else if (nums1[i] > nums2[j]) {
        //         ++j;
        //     } else {
        //         if (res.empty() || res.back() != nums1[i]) {
        //             res.push_back(nums1[i]);
        //         }
        //         ++i;
        //         ++j;
        //     }
        // }
        // return res;
        
        // sol3: sort, hashset, binary search
        unordered_set<int> res;
        sort(nums1.begin(), nums1.end());
        for (auto num : nums2) {
            if (binarySearch(num, nums1)) {
                res.insert(num);
            }
        }
        return vector<int>(res.begin(), res.end());
    }
    
    bool binarySearch(int target, vector<int> &nums) {
        int left = 0;
        int right = nums.size();
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return true;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return false;
    }
};
```

#### Java

```java
// sol1: hashset
// class Solution {
//     public int[] intersection(int[] nums1, int[] nums2) {
//         HashSet<Integer> st = new HashSet<>();
//         HashSet<Integer> res = new HashSet<>();
//         for (int num : nums1) {
//             st.add(num);
//         }
//         for (int num : nums2) {
//             if (st.contains(num)) {
//                 res.add(num);
//             }
//         }
//         return res.stream().mapToInt(i -> i).toArray();
//     }
// }

// sol2: sort, two pointers
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        int i1 = 0, i2 = 0;
        HashSet<Integer> res = new HashSet<>();
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        while (i1 < nums1.length && i2 < nums2.length) {
            if (nums1[i1] < nums2[i2]) {
                ++i1;
            } else if (nums1[i1] > nums2[i2]) {
                ++i2;
            } else {
                if (!res.contains(nums1[i1])) {
                    res.add(nums1[i1]);
                }
                ++i1;
                ++i2;
            }
        }
        return res.stream().mapToInt(i -> i).toArray();
    }
}
```
