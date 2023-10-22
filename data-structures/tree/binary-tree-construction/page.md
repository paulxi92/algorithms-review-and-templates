# Page

## Question

## Solution

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

// sol: recursion (use boundary)
class Solution {
    private int idx;

    public TreeNode bstFromPreorder(int[] preorder) {
        idx = 0;
        return helper(Integer.MIN_VALUE, Integer.MAX_VALUE, preorder);
    }

    private TreeNode helper(int lower, int upper, int[] preorder) {
        if (idx == preorder.length) {
            return null;
        } 
        int val = preorder[idx];
        if (val < lower || val > upper) {
            return null;
        }
        ++idx;
        TreeNode node = new TreeNode(val);
        node.left = helper(lower, val, preorder);
        node.right = helper(val, upper, preorder);
        return node;
    }
}
```
