## Leetcode 101. Symmetric Tree
这题不知道为何放在queue一节。。  
很直观的想法，就是判断左子树和右子树是否对称，递归的方式做下去。  
```java
class Solution {
  public boolean isSymmetric(TreeNode root) {
    if(root == null) {
      return true;
    }
    return helper(root.left, root.right);
  }
  private boolean helper(TreeNode one, TreeNode two) {
    if(one == null && two == null) {
      return true;
    } else if (one == null || two == null) {
      return false;
    } else if (one.val != two.val) {
      return false;
    }
    return helper(one.left, two.right) && helper(one.right, two.left);
  }
}
```
