[Link](https://www.lintcode.com/problem/rehashing/description)

```java
/**
 * Definition for ListNode
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @param hashTable: A list of The first node of linked list
     * @return: A list of The first node of linked list which have twice size
     */    
    public ListNode[] rehashing(ListNode[] hashTable) {
        // write your code here
        int capacity = hashTable.length * 2;
        ListNode[] results = new ListNode[capacity]; 
        for (int i = 0; i < hashTable.length; i++) {
            ListNode currentNode = hashTable[i];
            while (currentNode != null) {
                add(results, new ListNode(currentNode.val), capacity);
                currentNode = currentNode.next;
            }
        }
        return results;
    }
    void add(ListNode[] newNodes, ListNode node, int capacity) {
        int idx = hash(node.val, capacity);
        if (newNodes[idx] == null) {
            newNodes[idx] = node;
        } else {
            ListNode listNode = newNodes[idx];
            while (true) {
                if (listNode.next == null) {
                    listNode.next = node;
                    break;
                } else {
                    listNode = listNode.next;
                }
            }
        }
    }
    
    int hash(int val, int capacity) {
        if (val<0) {
           return (val % capacity + capacity) % capacity; 
        } else {
           return val % capacity;
        }
    }
};

```
