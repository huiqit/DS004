## Leetcode350. Intersection of Two Arrays
这题又是一类经典题，求2个或多个array的common elements.  
先来看在2个array的，首先 - clarification，和two sum的基本一样：  
* sorted or unsorted?
* data type?
* data size?
* optimize time or space? 
* duplicates?  

1. 如果sorted in ascending order --> **2 pointers 谁小移谁**
- Time = O(m+n)  
- Space = O(1)  
2. 如果两个array的size相差很大 --> **binary search**  
for each element in smaller array {  
   do binary search in the larger array  
}  
- Time = O(mlogn) (m << n)
- Space = O(1) 
3. 如果unsorted --> **HashSet**  
把一个array装进hashSet里，然后对另一个array的每个元素check。  
- Time = O(m+n)  
- Space = O(1)  
4. 如果要optimze for space呢？--> **先sort**  

所以这一题我们可以用solution3，或者4+1即先sort再2pointers。  
Leetcode这题有duplicates，所以Solution3只用一个hashSet是不够的，升级为hashMap:  
```java
// Runtime: 2 ms, faster than 91.28% of Java online submissions for Intersection of Two Arrays II.
// Memory Usage: 36.5 MB, less than 83.87% of Java online submissions for Intersection of Two Arrays II.
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if(nums1 == null || nums2 == null || nums1.length == 0 || nums2.length == 0) {
            return new int[]{};
        }
        List<Integer> resList = new ArrayList<>();
        
        Map<Integer, Integer> map = new HashMap<>();
        for(int n : nums1) {
            Integer count = map.getOrDefault(n, 0);
            count ++;
            map.put(n, count);
        }
        
        for(int n : nums2) {
            Integer count = map.get(n);
            if(count != null && count > 0) {
                resList.add(n);
                count --;
                map.put(n, count);
            }
        }
        
        int[] res = new int[resList.size()];
        for(int i = 0; i < res.length; i++) {
            res[i] = resList.get(i);
        }
        return res;
    }
}
```
```java
// Runtime: 2 ms, faster than 91.28% of Java online submissions for Intersection of Two Arrays II.
// Memory Usage: 36.3 MB, less than 83.87% of Java online submissions for Intersection of Two Arrays II.
class Solution4 {
  public int[] intersect(int[] nums1, int[] nums2) {
    if(nums1 == null || nums2 == null || nums1.length == 0 || nums2.length == 0) {
      return new int[]{};
    } 
    Arrays.sort(nums1);
    Arrays.sort(nums2);
    int p1 = 0;
    int p2 = 0;
    List<Integer> res = new ArrayList<>();
    
    while(p1 < nums1.length && p2 < nums2.length) {
      if(nums1[p1] == nums2[p2]) {
        res.add(nums1[p1]);
        p1++;
        p2++;
      } else if (nums1[p1] < nums2[p2]) {
        p1++;
      } else {
        p2++;
      }
    }
    int[] result = new int[res.size()];
    for(int i = 0; i < result.length; i++) {
      result[i] = res.get(i);
    } 
    return result;
  }
}
```
### Find common elements in 3 sorted arrays
很自然的把 2 pointers 拓展到 3 pointers.   
还是谁小移谁，移动min(A[i], B[j], C[k])，一次移动1个指针或者2个都行。  

### Find common elements in k sorted arrays 
相关问题: 
* merge k sorted array -> k way merge 
* k nodes in LCA  
这种k个array一起操作的方法，有  
* iterative way
   * Time = O(kn)
   * Space = O(n) 略有优势
* binary reduction 
   * 第一轮 O(kn)  
   * 第二轮 O(kn/2)  
   * 第三轮 O(kn/4)
   * ...
   * Total Time = kn*(1+1/2+1/4+...) = O(2kn) = O(kn) 
   * Space = O(kn)
* k way merge  
   * minHeap 无法generate出相同元素 还是要每个元素看一遍
   * not work well 
   * 能做，但是时间复杂度不会优于前两种方法 O(kn*logk) 




