# Page

## Question

## Solution

#### Java

```java
// sol: hashmap
class Solution {
    public int findShortestSubArray(int[] nums) {
        int n = nums.length;
        HashMap<Integer, Integer> lm = new HashMap<>(); // map number to left index
        HashMap<Integer, Integer> rm = new HashMap<>(); // map number to right index
        HashMap<Integer, Integer> cnts = new HashMap<>();
        int maxCnt = 0;
        int res = n;
        for (int i = 0; i < n; ++i) {
            int num = nums[i];
            lm.putIfAbsent(num, i);
            rm.put(num, i);
            cnts.put(num, cnts.getOrDefault(num, 0) + 1);
            maxCnt = Math.max(maxCnt, cnts.get(num));
        }
        for (int num : cnts.keySet()) {
            if (cnts.get(num) == maxCnt) {
                res = Math.min(res, rm.get(num) - lm.get(num) + 1);
            }
        }
        return res;
    }
}
```
