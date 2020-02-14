```java
class Solution {
    // 需要一个helper function可以generate从任意起点，到任意终点的List<TreeNode> 
    public List<TreeNode> generateTrees(int n) {
        if(n == 0) {
            return new ArrayList<>();
        }
        return generateTrees(1, n);
    }
    private List<TreeNode> generateTrees(int start, int end) {
        List<TreeNode> res = new ArrayList<>();
        if(start > end) {
            res.add(null);
            return res;
        }
        // 尝试每个点作为root
        for(int i = start; i <= end; i++) {
            
            List<TreeNode> left = generateTrees(start, i-1);
            List<TreeNode> right = generateTrees(i+1, end);
            
            for(TreeNode l : left) {
                for(TreeNode r : right) {
                    TreeNode root = new TreeNode(i); //必须要放在里面来，否则会有影响
                    root.left = l;
                    root.right = r;
                    res.add(root);
                }
            }
        }
        return res;
        
    }
}
```
