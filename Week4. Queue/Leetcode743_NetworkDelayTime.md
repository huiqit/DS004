## Leetcode 743. Network Delay Time
问从K点出发，最短多长时间能遍历所有node。  
很明显，用Djikstra算法，也就是把普通的BFS中的Queue换成PriorityQueue。  
```java
class Solution {
  public int networkDelayTime(int[][] times, int N, int K) {
        Map<Integer, Map<Integer, Integer>> map = new HashMap<>();
        for(int[] time : times) {
          map.putIfAbsent(time[0], new HashMap<>());
          map.get(time[0]).put(time[1], time[2]);
        }
        PriorityQueue<Integer[]> pq = new PriorityQueue<>((a, b) -> Integer.compare(a[0], b[0]));
        pq.offer(new Integer[]{0, K});
        boolean[] visited = new boolean[N+1];
        int res = 0;
        
        while(!pq.isEmpty()) {
          Integer[] curr = pq.poll();
          int currNode = curr[1];
          int currDist = curr[0];
          if(visited[currNode]) {
            continue;
          }
          visited[currNode] = true;
          res = currDist;
          N--;
          if(map.containsKey(currNode)) {
            for(int next : map.get(currNode).keySet()) {
              pq.offer(new Integer[]{currDist + map.get(currNode).get(next), next});
            }
          }
        }
        return N == 0 ? res : -1;
    }
}
