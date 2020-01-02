## Leetcode 42. Trapping Rain Water

直方图的蓄水问题，有外面积和内面积两类，这题是外面积问题，内面积就是[84题](https://leetcode.com/problems/largest-rectangle-in-histogram/)  
这题本身不难，难的是想到最优解，那我们先从primitive solution开始。  
input无非就是个一维array，那么扫一遍把每个点的蓄水量加起来就是了。  
那么每个点的蓄水量该怎么算呢？我们都知道木桶原理，这个点能蓄多少水取决于两边较低的那一边，注意，“两边”具体是什么，仔细想想or走个例子看看就知道应该是这边的最大值，那也就是min(leftMax, rightMax)，这样思路就出来了。别忘了还要减掉自身的高度才是蓄水量。  

那我们可以先预处理存下来leftMax & rightMax，再过一遍这个array算每个点的蓄水量。  
```java
// Runtime: 1 ms, faster than 93.90% of Java online submissions for Trapping Rain Water.
// Memory Usage: 36.9 MB, less than 100.00% of Java online submissions for Trapping Rain Water.
class Solution {
  public int trap(int[] height) {
    if(height == null || height.length == 0) {
      return 0;
    }
    int[] leftMax = new int[height.length];
    int[] rightMax = new int[height.length];
    leftMax[0] = height[0];
    rightMax[height.length-1] = height[height.length-1];
    for(int i = 1; i < height.length; i++) {
      leftMax[i] = Math.max(leftMax[i-1], height[i]);
    }
    for(int i = height.length-2; i >= 0; i--) {
      rightMax[i] = Math.max(rightMax[i+1], height[i]);
    }
    
    int sum = 0; // 总的蓄水量
    for(int i = 0; i < height.length; i++) {
      sum += Math.min(leftMax[i], rightMax[i]) - height[i];
    }
    return sum;
  }
}
```
这个方法时间已经是O(n)最优了，但是占用了O(n)的空间，我们想办法去优化空间。 
目标是找rightMax, leftMax，方法是two pointers 谁小移谁, 用两个变量记录rightMax, leftMax    
假设两头的柱子无法蓄水，然后站在当前位置看下一个柱子能蓄水多少，那就是leftMax/rightMax - 柱子高度  
证明：归纳法  
```java
class Solution {
// Runtime: 1 ms, faster than 93.66% of Java online submissions for Trapping Rain Water.
// Memory Usage: 37.2 MB, less than 98.63% of Java online submissions for Trapping Rain Water.
  public int trap(int[] height) {
    if(height == null || height.length == 0) {
      return 0;
    }
    int leftMax = height[0];
    int rightMax = height[height.length-1];
    int i = 0;
    int j = height.length-1;
    int sum = 0;
    while(i < j) {
      // 先更新这俩
      leftMax = Math.max(leftMax, height[i]);
      rightMax = Math.max(rightMax, height[j]);
      // 再计算下一个小柱子的蓄水量，并移动i, j
      if(height[i] < height[j]) {
        sum += leftMax - height[i];
        i++;
      } else {
        sum += rightMax - height[j];
        j--;
      }
    }
    return sum;
  }
}
```

