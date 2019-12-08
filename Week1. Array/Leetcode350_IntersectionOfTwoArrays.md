## Leetcode350. Intersection of Two Arrays
这题又是一类经典题，求2个或多个array的common elements.  
先来看在2个array的，首先，clarification，和two sum的基本一样：  
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
```java
// Runtime: 2 ms, faster than 91.28% of Java online submissions for Intersection of Two Arrays II.
// Memory Usage: 36.3 MB, less than 83.87% of Java online submissions for Intersection of Two Arrays II.
class Solution4+1 {
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



