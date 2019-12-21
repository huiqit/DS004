## Leetcode 42. Trapping Rain Water

直方图的蓄水问题，有外面积和内面积两类，这题是外面积问题，内面积就是[84题](https://leetcode.com/problems/largest-rectangle-in-histogram/)  
这题本身不难，难的是想到最优解，那我们先从primitive solution开始。  
input无非就是个一维array，那么扫一遍把每个点的蓄水量加起来就是了。  
那么每个点的蓄水量该怎么算呢？我们都知道木桶原理，这个点能蓄多少水取决于两边较低的那一边，注意，“两边”具体是什么，仔细想想or走个例子看看就知道应该是这边的最大值，那也就是min(leftMax, rightMax)，这样思路就出来了。  

