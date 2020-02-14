```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] res = new int[numCourses];
        int[] indegree = new int[numCourses];
        
        // get the indegree for each course
        for(int[] pre : prerequisites) {
            indegree[pre[0]] ++;
        }
        
        // put indegree == 0 's course to queue
        Queue<Integer> queue = new ArrayDeque<>();
        for(int i = 0; i < numCourses; i++) {
            if(indegree[i] == 0) {
                queue.offer(i);
            }
        }
        // execute the course
        int i = 0;
        while(!queue.isEmpty()) {
            Integer curr = queue.poll();
            res[i++] = curr;
            
            // remove the pre = curr
            for(int[] pre : prerequisites) {
                if(pre[1] == curr) {
                    indegree[pre[0]] --;
                    if(indegree[pre[0]] == 0) {
                        queue.offer(pre[0]);
                    }
                }
            }
        }
        
        
        return i == numCourses ? res : new int[]{};
    }
}
```
