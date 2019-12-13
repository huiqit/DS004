## Leetcode142. LinkedList Cycle II
linked list中关于cycle的题有2道，[141](https://leetcode.com/problems/linked-list-cycle/)是问有没有环，返回boolean的，[142](https://leetcode.com/problems/linked-list-cycle-ii/)是问如果有环起点在哪，返回int。还有个应用是[187](https://leetcode.com/problems/find-the-duplicate-number/) Find duplicate number。    
我们先看第一题，简单一点，经典方法是用快慢指针。  
思路：快指针一次走2步，慢指针一次走1步，如果两指针相遇，就意味着有环，否则快指针会先走到null。  
```java
// Runtime: 0 ms, faster than 100.00% of Java online submissions for Linked List Cycle.
// Memory Usage: 37.4 MB, less than 98.57% of Java online submissions for Linked List Cycle.
class Solution {
  public boolean hasCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while(fast != null && fast.next != null) {
      slow = slow.next;
      fast = fast.next.next;
      if(slow == fast) {
        return true;
      }
    }
    return false;
  }
}
```

第二题思路稍微难点，是龟兔赛跑算法。  
首先，slow fast 从头开始走，相遇的那点，记为M；  
然后，再用2个指针，一个从头开始走，一个从M开始走，相遇点即为cycle的起点。  
这个证明画个图标注一下长度就ok了。  
```java
class Solution {
  public ListNode cycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while(fast != null && fast.next != null) {
      slow = slow.next;
      fast = fast.next.next;
      if(slow == fast) {
        // 找到了相遇点，就是slow = fast这点
        while(head != slow) {
          head = head.next;
          slow = slow.next;
        }
        return slow;
      }
    }
    return null;
  }
}
``` 

