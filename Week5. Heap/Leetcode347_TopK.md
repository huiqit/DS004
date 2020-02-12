## Leetcode 347. Top K Frequent Elements
题目：给一组数，找到出现最多的前k个。  
思路：按frequency排个序，然后再取前k个。

'''java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int n : nums) {
            Integer count = map.getOrDefault(n, 0);
            count++;
            map.put(n, count);
        }

        PriorityQueue<Map.Entry<Integer, Integer>> minHeap = new PriorityQueue<>(k+1, new Comparator<Map.Entry<Integer, Integer>>() {
           @Override
            public int compare(Map.Entry<Integer, Integer> e1, Map.Entry<Integer, Integer> e2) {
                return Integer.compare(e1.getValue(), e2.getValue());
            }
        });
        
        for(Map.Entry<Integer, Integer> entry : map.entrySet()) {
            minHeap.offer(entry);
            if(minHeap.size() > k) {
                minHeap.poll();
            }
        }
        List<Integer> res = new ArrayList<>();
        while(!minHeap.isEmpty()) {
            res.add(minHeap.poll().getKey());
        }
        Collections.reverse(res);
        return res;
    }
}
'''
