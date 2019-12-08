## Leetcode4. Median of Two sorted arrays
根据题干，2个sorted array且要求时间复杂度为O(log(m+n))，那自然是不能够简单的过一遍了，而应该利用到sorted的性质，根据这个logn的时间复杂度很自然想到了binary search，能否每次排除一半的元素呢？
我们把nums1和nums2合在一起来看，找它们俩的median无非也就是合并一起的大的array的中位数。  
而中位数的计算depends on这个array的奇偶个数，所以需要得到第(m+n)/2 & (m+n)/2 + 1 的数。  
那作为helper function，我们自然把它记为第k大的元素，此题中 k = (m+n)/2。  

思路：
two pointers. 谁小移谁，直到移了k次。O(k) → how to get O(logk)：  
利用binary search的思想，每次删掉一半一定不包含target的元素，以此来缩小搜索范围  
int[] X = xxxxxxxxxx      k = 10 → 5 → 2 → 1  
int[] Y = yyyyyyyyyy  
每次比较XY中第k/2个元素，谁小删除谁，因为第k小的元素一定不在小的那边，证明：  
反证法，假设X[k/2-1] < Y[k/2-1] 且 第k小的元素在X array里，设第k小的元素的index为j，那么：  
X[j] <= X[k/2-1] < Y[k/2-1]，  
所以比k小的元素最多只能在X[0...j-1] & Y[0...k/2-2]里（高亮的部分）  
这些元素<=k-2个，但是比k小的元素应该有k-1个，所以矛盾。  

具体做法：  
挡板法，不需要真的删除谁，而是用left + mid的形式确定搜索空间。  
每次取k/2，同质的问题所以用recursion做。  
起点设为aleft, bleft，终点设为amid, bmid  

base case :   
k == 1时，return a b 中较小的那一个，因为k→1 和 1→k是一样的。  
另外，需要boundary check. a用完了，那剩下的就直接取b里的；b同理。  
recursion rule:   
比较amid和bmid的值的大小，谁小，那就下次recursion的时候小的那个的起点设为mid+1（因为mid已经比较过了，所以要+1）  
在这之前，要注意：  
amid = aleft + k/2 - 1; -1 因为是index  
算好了index要有boundary check，如果越界了那就设为max，因为要保留a里的数。(tips)  
假设a array有3个数，b里有10个数，找第10小的那不可能在b的前5个元素里，因为b的前5个+a全部才8个数，所以一定能把b的前k/2删掉，那就把a的这个值设为最大值来保证a不被删掉。  

```java
// Runtime: 2 ms, faster than 99.97% of Java online submissions for Median of Two Sorted Arrays.
// Memory Usage: 46.3 MB, less than 91.67% of Java online submissions for Median of Two Sorted Arrays.
class Solution {
  public double findMedian(int[] nums1, int[] nums2) {
    int len = nums1.length + nums2.length;
    if(len % 2 == 0) {
      return (kth(nums1, nums2, 0, 0, len/2) + kth(nums1, nums2, 0, 0, len/2 + 1)) / (double)2;
    } else {
      return kth(nums1, nums2, 0, 0, len/2 + 1);
    }
  }
  private int kth(int[] a, int[] b, int aleft, int bleft, int k) {
    if(aleft >= a.length) {
      return b[bleft + k - 1];
    }
    if(bleft >= b.length) {
      return a[aleft + k - 1];
    } 
    if(k == 1) {
      return Math.min(a[aleft], b[bleft]);
    } 
    int amid = aleft + k / 2 - 1;
    int bmid = bleft + k / 2 - 1;
    int aval = amid >= a.length ? Integer.MAX_VALUE : a[amid];
    int bval = bmid >= b.length ? Integer.MAX_VALUE : b[bmid];
    
    if(aval <= bval) {
      return kth(a, b, amid + 1, bleft, k - k / 2);
    } else {
      return kth(a, b, aleft, bmid + 1, k - k / 2);
    }
  }
}
```
