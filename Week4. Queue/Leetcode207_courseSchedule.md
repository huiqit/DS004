## Leetcode 207. Course Schedule 
这题是典型的拓扑排序，从degree=0的开始，用一个queue maintain顺序，不多说了直接看代码吧。  
```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        
        int[] indegree = new int[numCourses];
        int count = 0;
        
        // get the indegree of each course
        for(int[] pre : prerequisites) {
            indegree[pre[0]]++;
        }
        // get those indegree == 0 to queue
        Queue<Integer> queue = new ArrayDeque<>();
        for(int i = 0; i < numCourses; i++) {
            if(indegree[i] == 0) {
                queue.offer(i);
            }
        }
        // excute the course
        while(!queue.isEmpty()) {
            Integer curr = queue.poll();
            count++;
            // remove the indegree with prere = curr
            for(int[] pre : prerequisites) {
                
                if(pre[1] == curr) {
                    indegree[pre[0]] --;
                    if(indegree[pre[0]] == 0) {
                        queue.offer(pre[0]);
                    }
                }
            }
        }
        
        
        return count == numCourses;
    }
}
```
