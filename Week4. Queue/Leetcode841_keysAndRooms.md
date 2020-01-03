## Leetcode 841. Keys and Rooms 
这题不就是拓扑排序吗？为啥tag里没写。。  
拓扑排序是一个DAG图的线性排列，一个图可以有多种排列方式。  
实现方法DFS或者BFS都行。  

具体到这题来说，DFS深度优先搜索那就是用stack，可以用recursion的隐形stack来做。  
维护一个hashSet来存放visited过的房间，从start room开始，把能够visited的房间都放进去，最后比较这个set的数量和room的数量，如果两者相等返回true，否则false。这里hashSet还兼顾了去重的功能，一举两得。  
代码写起来也非常简介。  
```java
// Runtime: 1 ms, faster than 91.57% of Java online submissions for Keys and Rooms.
// Memory Usage: 42.4 MB, less than 100.00% of Java online submissions for Keys and Rooms.

class Solution {
  public boolean canVisitAllRooms(List<List<Integer>> rooms) {
    Set<Integer> set = new HashSet<>();
    set.add(0);
    dfs(0, rooms, set);
    return set.size() == rooms.size();
  }
  
  private void dfs(int num, List<List<Integer>> rooms, Set<Integer> set) {
    for(int key : rooms.get(num)) {
      if(set.add(key)) {
        dfs(key, rooms, set);
      }
    }
  }
}
```
BFS的方法就是用queue了，把可以visited且未被visited过的房间放入queue，然后一个个poll出来，直到为空。  
```java
// Runtime: 2 ms, faster than 50.51% of Java online submissions for Keys and Rooms.
// Memory Usage: 42.5 MB, less than 100.00% of Java online submissions for Keys and Rooms.

class Solution {
  public boolean canVisitAllRooms(List<List<Integer>> rooms) {
    Set<Integer> set = new HashSet<>();
    bfs(0, rooms, set);
    return set.size() == rooms.size();
  }
  private void bfs(int num, List<List<Integer>> rooms, Set<Integer> set) {
    Queue<Integer> queue = new ArrayDeque<>();
    queue.offer(num);
    set.add(num);
    while(!queue.isEmpty()) {
      int curr = queue.poll();
      set.add(curr);
      for(int key : rooms.get(curr)) {
        if(set.add(key)) {
          queue.offer(key);
        }
      }
    }
  }
}
```
