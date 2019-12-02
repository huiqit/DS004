## 15. 3Sum  
题意：在给定的array里找到3个数，使得它们之和等于0. 返回所有组合。  
[Link](https://leetcode.com/problems/3sum/)  
思路：在2Sum的基础上，其实就是把target当作了0-a，即要找到 b + c = 0 - a.  
那么就两层循环，对每个a，在它后面找是否有合适的b, c  
代码如下：
```java
class Solution {
  public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if(nums == null || nums.length == 0) {
      return res;
    }
    
  
  }
}
```
