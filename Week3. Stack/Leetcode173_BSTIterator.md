## Leetcode 173. BST Iterator
这题就是一个披着羊皮的in order traversal，都不是啥Iterator...  
初始化的时候就把最左边一排都加进去，next() API里面保证stack里一直是最左边的数。  

```java
class BSTIterator {
  private final Deque<TreeNode> stack;
  
  public BSTIterator(TreeNode root) {
    stack = new ArrayDeque<>();
    pushLeft(root);
  }
  
  private void pushLeft(TreeNode curr) {
    while(curr != null) {
      stack.offerFirst(curr);
      curr = curr.left;
    }
  }
  
  public int next() {
    if(!hasNext()) {
      throw new NoSuchElementException("no more element!");
    }
    TreeNode curr = stack.pollFirst();
    pushLeft(curr.right);
    return curr.val;
  }
  
  public boolean hasNext() {
    return !stack.isEmpty();
  }
}

```
