## Leetcode203. Remove LinkedList Element
Remove all elements from a linked list of integers that have value val.  
就是在这个LL里把某个值删掉。  
思路：过一遍这个LinkedList，遇到这个value时，就跳过它。  
操作：  
1. 用一个dummyHead，因为有可能原来的head就是这个value，那就不能返回之前的head了，甚至可能返回null；
2. 用几个指针：
  * prev: 最后要返回的linked list的tail；prev之前是要保留的那些listNode
  * curr: 在iterate原array的当前node；
3. 操作：
  * 如果curr的值等于val，那就继续直接看下一个；
  * 如果不相等，也就是要保留这个值，设置prev.next = curr，然后两个指针都要移动到下一个。
Time = O(n)  
Space = O(1)  

```java
// Runtime: 1 ms, faster than 98.69% of Java online submissions for Remove Linked List Elements.
// Memory Usage: 40 MB, less than 100.00% of Java online submissions for Remove Linked List Elements.
class Solution {
  public ListNode remove(ListNode head, int val) {
    ListNode dummyHead = new ListNode(0);
    ListNode prev = dummyHead;
    ListNode curr = head;
    while(curr != null) {
      if(curr.val != val) {
        prev.next = curr;
        prev = prev.next;
        curr = curr.next;
      } else {
        curr = curr.next;
      }
    }
    prev.next = null;
    return dummyHead.next;
  }
}
```
