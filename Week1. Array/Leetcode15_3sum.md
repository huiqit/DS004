## Leetcode15. 3Sum  
题意：在给定的array里找到3个数，使得它们之和等于0. 返回所有组合。  
[Link](https://leetcode.com/problems/3sum/)  

这题是基于大名鼎鼎的two sum来做的，那么我们先来看一下two sum。
### Two Sum
这题有很多问法depends on题目给的条件是什么，所以面试时要先clarify问题：
* duplicates?
* sorted?
* input data type? 
* input data size? -> memory or disk -> 升级system design问题
* 优化空间or时间？
* return index or value or boolean? 如果有多组解，是都return还是任意一个？
...  
  
最常见的：对于unsorted的int array，没有duplicates且只有唯一一个解的来说，也就是[Leetcode 第一题](https://leetcode.com/problems/two-sum/)  
#### unsorted, no duplicates  
最最基本的想法，就是锁定一个数，在它后面找另一个呗，那就是两层循环：  
Time complexity = O(n^2)  
Space = O(1)  
运行结果也确实如此：  
Runtime: 19 ms, faster than 30.26% of Java online submissions for Two Sum.  
Memory Usage: 37.2 MB, less than 98.95% of Java online submissions for Two Sum.  
```java
class Solution {
  public boolean twoSum(int[] array, int target) {
    for(int i = 0; i < array.length; i++) {
      for(int j = i + 1; j < array.lengthl; j++) {
        if(array[i] + array[j] == target) {
            return new int[]{i, j};
        }
      }
    }
    return new int[]{-1, -1};
  }
}
```
此时空间达到了最优，所以我们想看看还能否优化时间呢？能否iterate一次就得到解呢？  
上述方法是锁定一个nums[i] = x 去找后面有没有能够和他搭伴的target - x，其实后面这些数被check了很多遍啊，换一个x就要重新再
比如1 2 3 4 5，x = 1的时候，我们check了y = 2, 3, 4, 5 能否x + y == target；  
都不行了，换 x = 2，check y = 3, 4, 5 ... 所以才造成了O(n^2)的时间，那如果能用O(1)的时间check，就可以把时间缩到O(n)了。  

O(1) check --> HashSet! you got it!
所以边走边check（让我想到了buy and sell stock的第一题，同样的思路优化到O(n)，边走边记录min so far）  
所以这里hashSet represents the value we have seen so far, 不包括当前的值（因为要找自己+之前的==target）   
但Leetcode这题要求返回index，所以我们只用hashSet记录value是不够的，升级为hashMap记录<value, index>.   
Time = O(n)  
Space = O(n)  
Runtime: 2 ms, faster than 98.95% of Java online submissions for Two Sum.  
Memory Usage: 38.1 MB, less than 95.28% of Java online submissions for Two Sum.  
```java
class Solution {
  public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for(int i = 0; i < nums.length; i++) {
      Integer indexPrev = map.get(target - nums[i]);
      if(indexPrev != null) {
        return new int[]{indexPrev, i};
      }
      map.put(nums[i], i);
    }
    return new int[]{-1, -1};
  }
}
```

### 3Sum
思路：在2Sum的基础上，其实就是把target当作了0-a，即要找到 b + c = 0 - a.  
那么就两层循环，对每个a，在它后面找是否有合适的b, c  
Time = O(n^2)  
Space = O(n)  
```java
class Solution {
  public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if(nums == null || nums.length == 0) {
      return res;
    }
    Arrays.sort(nums);
    for(int i = 0; i < nums.length - 2; i++) {
      // find 
      Set<Integer> set = new HashSet<>();
      for(int j = i + 1; j < nums.length - 1; j++) {
        if(set.contains(-nums[i]-nums[j])) {
          res.add(new ArrayList<>(nums[i], -nums[i]-nums[j], nums[j]));
        }
        set.add(nums[j]);
      }
    }
    return res;
  }
}
```
