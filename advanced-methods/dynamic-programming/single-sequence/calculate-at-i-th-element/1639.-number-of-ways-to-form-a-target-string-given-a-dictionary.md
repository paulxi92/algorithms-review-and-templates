# Page

## Question

You are given a list of strings of the **same length** `words` and a string `target`.

Your task is to form `target` using the given `words` under the following rules:

* `target` should be formed from left to right.
* To form the `ith` character (**0-indexed**) of `target`, you can choose the `kth` character of the `jth` string in `words` if `target[i] = words[j][k]`.
* Once you use the `kth` character of the `jth` string of `words`, you **can no longer** use the `xth` character of any string in `words` where `x <= k`. In other words, all characters to the left of or at index `k` become unusuable for every string.
* Repeat the process until you form the string `target`.

**Notice** that you can use **multiple characters** from the **same string** in `words` provided the conditions above are met.

Return _the number of ways to form `target` from `words`_. Since the answer may be too large, return it **modulo** `109 + 7`.

**Example 1:**

<pre><code><strong>Input: words = ["acca","bbbb","caca"], target = "aba"
</strong><strong>Output: 6
</strong><strong>Explanation: There are 6 ways to form target.
</strong>"aba" -> index 0 ("acca"), index 1 ("bbbb"), index 3 ("caca")
"aba" -> index 0 ("acca"), index 2 ("bbbb"), index 3 ("caca")
"aba" -> index 0 ("acca"), index 1 ("bbbb"), index 3 ("acca")
"aba" -> index 0 ("acca"), index 2 ("bbbb"), index 3 ("acca")
"aba" -> index 1 ("caca"), index 2 ("bbbb"), index 3 ("acca")
"aba" -> index 1 ("caca"), index 2 ("bbbb"), index 3 ("caca")
</code></pre>

**Example 2:**

<pre><code><strong>Input: words = ["abba","baab"], target = "bab"
</strong><strong>Output: 4
</strong><strong>Explanation: There are 4 ways to form target.
</strong>"bab" -> index 0 ("baab"), index 1 ("baab"), index 2 ("abba")
"bab" -> index 0 ("baab"), index 1 ("baab"), index 3 ("baab")
"bab" -> index 0 ("baab"), index 2 ("baab"), index 3 ("baab")
"bab" -> index 1 ("abba"), index 2 ("baab"), index 3 ("baab") 
</code></pre>

**Constraints:**

* `1 <= words.length <= 1000`
* `1 <= words[i].length <= 1000`
* All strings in `words` have the same length.
* `1 <= target.length <= 1000`
* `words[i]` and `target` contain only lowercase English letters.

## Solution

#### Java

```java
// sol1: backtracking with memory
// class Solution {
//     private long[][] mem;
//     private int[][] freq;
//     private int M = (int) 1e9 + 7;

//     public int numWays(String[] words, String target) {
//         int m = target.length(), n = words[0].length();
//         mem = new long[m][n]; // mem[i][j]: number of ways for target.substring(i) with current column j
//         freq = new int[26][n]; // freq[i][j]: frequency of char i in column j
//         for (long[] v : mem) {
//             Arrays.fill(v, -1);
//         }
//         for (int col = 0; col < n; ++col) {
//             for (String word : words) {
//                 ++freq[word.charAt(col) - 'a'][col];
//             }
//         }
//         return (int) helper(0, 0, n, target);
//     }

//     private long helper(int cur, int col, int n, String target) {
//         int m = target.length();
//         if (cur == m) {
//             return 1;
//         }
//         if (col == n) {
//             return 0;
//         }
//         if (mem[cur][col] != -1) {
//             return mem[cur][col];
//         }
//         mem[cur][col] = helper(cur, col + 1, n, target) + freq[target.charAt(cur) - 'a'][col] * helper(cur + 1, col + 1, n, target);
//         return mem[cur][col] % M;
//     }
// }

// sol2: 2D dp
class Solution {
    public int numWays(String[] words, String target) {
        int m = target.length(), n = words[0].length();
        long[][] dp = new long[m + 1][n + 1]; // dp[i][j]: number of ways for target.substring(0, i) with current column j - 1
        int[][] freq = new int[26][n]; // freq[i][j]: frequency of char i in column j
        int M = (int) 1e9 + 7;
        for (int col = 0; col < n; ++col) {
            for (String word : words) {
                ++freq[word.charAt(col) - 'a'][col];
            }
        }
        for (int col = 0; col <= n; ++col) {
            dp[0][col] = 1;
        }
        for (int cur = 1; cur <= m; ++cur) {
            for (int col = 1; col <= n; ++col) {
                dp[cur][col] = dp[cur][col - 1] + freq[target.charAt(cur - 1) - 'a'][col - 1] * dp[cur - 1][col - 1];
                dp[cur][col] %= M;
            }
        }
        return (int) dp[m][n];
    }
}
```
