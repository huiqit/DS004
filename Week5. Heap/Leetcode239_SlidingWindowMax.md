
```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums == null || nums.length == 0 || k > nums.length) {
            return new int[]{};
        }
        int[] res = new int[nums.length - k + 1];
        Deque<Integer> deque = new ArrayDeque<>();
        for(int i = 0; i < nums.length; i++) {
            while(!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }
            if(!deque.isEmpty() && deque.peekFirst() <= i-k) {
                deque.pollFirst();
            }
            deque.offerLast(i);
            if(i >= k-1) {
                res[i-k+1] = nums[deque.peekFirst()];
            }
        }
        return res;
    }
}
```
