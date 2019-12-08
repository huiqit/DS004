## Leetcode33. Search in Rotated Sorted Array
这题是binary search的变种了，对binary search的核心思想有了更深刻的理解，曾在高盛电面中被问到。  
Binary Search的核心思想是每次想办法去掉一半元素，且保证target不在这一半元素里。  
关于search in rotated sorted array有以下相关问题：  
- 求最小值。index, value. 
  - follow up: duplicates. 区别在于mid == right时无法判断min在哪边了，所以只能一步步移动，无法guarantee log(n), worst case O(n). 
- 求target的index/ 是否在里面。LC33 & LC81
  - follow up: duplicates. 

**思路**：想办法去掉一半，一般是通过比较mid和左右两端的关系。

### Q1. 求最小值and no duplicates [LC 153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

比较array[mid] 和 两端的大小关系，哪边都行。

先看和右边的比较：（以下用mid代替array[mid]，用right代替array[right]）

- 如果mid > right，这是不正常的（不sorted）的，说明最小值在右边，所以排出左边以及mid（mid已经比right大了，不可能为最小值）left = mid+1；

- 如果mid < right，说明右边是sorted的，说明最小值在左边甚至可能就是mid本身，所以right = mid。
- 这样的话，while终止条件可以是left == right的时候，此时mid = left，left 和 right还会进行比较以及移动，所以可以跳出循环
  - 跳出循环时，left == right == mid，那就返回nums[left] = nums[right] 任意都行。

```java
class Solution{
  public int findMid(int[] nums) {
    if(nums == null || nums.length == 0) {
      return -1; // discuss with interviewer
    }
    int left = 0;
    int right = nums.length - 1;
    while(left < right) {
      int mid = left + (right - left)/2;
      if(nums[mid] > nums[right]) {
        left = mid + 1;
      } else {
        right = mid;
      }
    }
    return nums[left];
  }
}
/*
Runtime: 0 ms, faster than 100.00% of Java online submissions for Find Minimum in Rotated Sorted Array.
Memory Usage: 38.5 MB, less than 79.54% of Java online submissions for Find Minimum in Rotated Sorted Array.
@Nov 27 2019
*/
```

再看和左边比较：

- 如果mid >**=** left，那么左边是sorted的，最小值在右边（假设一定rotated了），left = mid+1;
  - 注意⚠️：最小值有可能就是left，虽然最初数组是rotate了，但是在减半之后有可能就是完整sorted的
  - 所以此方法还要加一步：最开始先判断是否是sorted的，即array[left] vs array[right]
  - 注意⚠️：mid = left时，只剩2个元素，且上一步判断过了这2个元素并非sorted，所以最小值是right，所以需要移动left = mid+1。和right比较不需要，因为mid不会等于right。
- 如果mid < left，那么右边是sorted的，最小值在左边甚至是mid本身，right = mid. 
- while loop条件同上

```java
class Solution {
  public int findMid(int[] nums) {
    int left = 0;
    int right = nums.length - 1;
    while(left < right) {
      if(nums[left] < nums[right]) { //⚠️
        return nums[left];
      }
      int mid = left + (right - left)/2;
      if(nums[mid] >= nums[left]) { //⚠️
        left = mid + 1;
      } else {
        right = mid;
      }
    }
    return nums[left];
  }
}
/*
Runtime: 0 ms, faster than 100.00% of Java online submissions for Find Minimum in Rotated Sorted Array.
Memory Usage: 38.6 MB, less than 77.27% of Java online submissions for Find Minimum in Rotated Sorted Array.
*/
```

### Q2. 求最小值and may contain duplicates [LC 154](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

基本思路和Q1一样，但是[3, 3, 1, 3] 不work，因为当mid == right时，无法判断min到底在哪边，所以只能一步步走，worst case O(n).

```java
class Solution {
  public int findMin(int[] nums) {
    int left = 0;
    int right = nums.length - 1;
    while(left < right) {
      int mid = left + (right - left)/2;
      if(nums[mid] > nums[right]) {
        left = mid + 1;
      } else if (nums[mid] == nums[right]) {
        right --;
      } else {
        right = mid;
      }
    }
    return nums[left];
  }
}
/*
Runtime: 0 ms, faster than 100.00% of Java online submissions for Find Minimum in Rotated Sorted Array II.
Memory Usage: 41.8 MB, less than 6.25% of Java online submissions for Find Minimum in Rotated Sorted Array II.
*/
```

### Q3. 找target [LC33](https://leetcode.com/problems/search-in-rotated-sorted-array/)

这题有很多方法，这里分享个人觉得最好用的一种。

一个数组rotate之后，其实是2段分别sorted的数组，那么从中间分开，必然有一半是sorted的，另一半是rotate的，所以：

1. 先判断这边是不是sorted的：array[left] <= array[mid]，必须写等号，因为mid可能等于left
2. 在sorted的那边，判断target是否在这个区间内，相应的移动left, right

注意⚠️：这里while条件一定是等于，否则最后left==right的时候就跳出去了，然而我们不能像上一题那样跳出去return nums[left]，因为有可能不存在呀！上一题找最小值一定是存在的。所以return一定是在while循环里面，出去了说明没有这个target，所以一个元素的时候也需要进入循环，所以进入循环的条件就是left <= right. 

easy way to find the condition: {1}, target = 1. 

```java
// return index
class Solution {
  public int search(int[] nums, int target) {
    if(nums == null || nums.length == 0) {
      return -1;
    }
    int left = 0;
    int right = nums.length - 1;
    while(left <= right) { 
      int mid = left + (right - left)/2;
      if(nums[mid] == target) {
        return mid;
      }
      if(nums[left] <= nums[mid]) { // left part is sorted
        if(target >= nums[left] && target <= nums[mid]) { // target is in the left part
          right = mid - 1;
        } else {
          left = mid + 1;
        }
      } else { // right part is sorted
        if(target >= nums[mid] && target <= nums[right]) { // target is in the right part
          left = mid + 1;
        } else {
          right = mid - 1;
        }
      }
    }
    return -1; // does not find
  }
}
```

### Q4. 找target & duplicates [LC81](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

和Q2一样，如果nums[mid] == nums[left] 无法判断sorted

那如果 判断nums[mid] == nums[right] 呢？[1, 1, 3, 1], 3. 

也就是当nums[mid] == nums[left/right]时，无法判断target在哪边，所以无论和left or right比较，只要相等了，就只能一步步走。

```java
class Solution {
    public boolean search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while(left <= right) {
            int mid = left + (right - left)/2;
            if(nums[mid] == target) {
                return true;
            }
            if(nums[mid] == nums[right]) { // mid不是target，所以right也不是
                right--;
            } else if(nums[mid] < nums[right]) {
                if(target >= nums[mid] && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            } else {
                if(target >= nums[left] && target <= nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
        }
        return false;
    }
}
```

### 总结

更深刻的理解了binary search，如何寻找target不在的那一半是关键。

通过比较mid和两端的关系来判断。

