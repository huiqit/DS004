## Leetcode83. Remove duplicates in sorted list
这是很经典的一个系列：remove duplicates，可以在linkedlist里做，也可以在array里做。问法多种多样，比如重复元素只保留1个，保留2个，一个都不保留，还有消消乐那种模式的。  
这类题都是用快慢指针来做的，但注意每道题里面指针的物理意义不同。  
这道题：  
slow pointer: 左边及包括自己就是要保留的元素  
fast pointer: iterate the array   
```java
// Runtime: 0 ms, faster than 100.00% of Java online submissions for Remove Duplicates from Sorted List.
// Memory Usage: 36.8 MB, less than 100.00% of Java online submissions for Remove Duplicates from Sorted List.
class Solution {
  public ListNode deleteDuplicates(ListNode head) {
    if(head == null) {
      return null;
    }
    ListNode slow = head;
    ListNode fast = head;
    while(fast != null) {
      if(fast.val == slow.val) {
        fast = fast.next;
      } else {
        slow.next = fast;
        slow = slow.next;
        fast = fast.next;
      }
    }
    slow.next = null;
    return head;
  }
}
```
