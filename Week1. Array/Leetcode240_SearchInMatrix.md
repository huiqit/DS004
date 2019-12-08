## Leetcode240. Search a 2D Matrix II  
关于在matrix里搜索有2道题：  
1. [Leetcode74](https://leetcode.com/problems/search-a-2d-matrix/) 这里的matrix是严格sorted的，即每一行的最后一个元素的值都会小于下一行的第一个值，俨然一个sorted array，那就可以用经典binary search来做；  
2. [Leetcode240](https://leetcode.com/problems/search-a-2d-matrix-ii/) 这里的matrix并非严格sorted，但也遵循了一定的规律，基于这个规律我们每次也可以排除多个元素，达到 **binary** 的效果  

直接看这个题吧，如何排除多个元素呢？  
* 如果当前元素是8，target是20，那我们可以排除掉target的**左边**和**上边**包围的这些元素，因为这些元素必然小于8；  
* 如果target小于当前元素呢，那我们可以排除掉target的**右边**和**下边**及其包围的这些元素，因为这些元素大于当前元素。  

另外，我们有这样的observations: 左上角的是最小值，右下角的是最大值，那么如果从这两个角出发，不确定下一步的方向，因为有2个方向都是ok的，所以，我们选择从另外两个角出发，这样能够每次有确定的方向。比如  
* 从右上角出发，它的左边比它小，它的下边比他大，而我们只能二选一走其中一个；  
* 从左下角出发，它的上边比他小，它的右边比他大，二选一方向确定。  
Runtime: 5 ms, faster than 100.00% of Java online submissions for Search a 2D Matrix II.  
Memory Usage: 42.6 MB, less than 100.00% of Java online submissions for Search a 2D Matrix II.  

```java
class Solution {
  public boolean searchMatrix(int[][] matrix, int target) {
    if(matrix == null || matrix.length == 0 || matrix[0].length == 0) {
      return false;
    }

    int i = 0; 
    int j = matrix[0].length - 1;
    while(i < matrix.length && j >= 0) {
      if(matrix[i][j] == target) {
        return true;
      } else if (target > matrix[i][j]) {
        i++;
      } else {
        j--;
      }
    }
    return false;
  }
}
```
