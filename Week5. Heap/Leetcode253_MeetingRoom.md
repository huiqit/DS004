## Leetcode 253. Meeting Room
关于 meeting room 有2道题，一题是252，另一题就是253了。  
题干都是给几组数字，情景就是这些数字是会议的起始时间，比如：[[0,30],[5,10],[15,20]] 的意思就是第一个会议从 0 到 30，第二个会议从 5 到 10，第三个会议从15 到 20，至于数字的单位是什么，不必追究。  
252 是让判断某人是否能同时参加这些会议，其实也就是判断会议时间有没有重叠。这个和直观上我们判断的方法是一样的，就是查一下有没有重叠了的时间，也就是下一个meeting 的开始时间不能比前一个 meeting 的结束时间还早。  
具体的做法就是先按照开始时间排个序，然后查看下一个的开始时间是否比前一个的结束时间早。  
这里的排序用了个函数式编程，大大简化了代码量，感兴趣的童鞋可以后台留言之后我们可以单独说一下 Lambda 表达式，也是 Java8 的重要新增特性。  

```java
class Solution {
  public boolean canAttendMeetings(int[][] intervals) {
    Arrays.sort(intervals, (x, y) -> (x[0] - y[0]));
    for(int i = 1; i < intervals.length; i++) {
      if(intervals[i][0] < intervals[i-1][1]) {
        return false;
      }
    }
    return true;
  }
}
```

再来看253题，在上一题的基础上，要求求出至少需要几个会议室。这题有多种解法，大家先自己想想。

我一开始的想法是，在上一题的基础上，把 return false 那里改成每次 + 1 不就行了，但再一想，不对，有的其实可以一起。  
上一题只是判断了相邻的两个会议是否有重叠时间，是一种比较严格的条件，这题如果 A 和 B 有重叠，B 和 C 有重叠，但是 A 和 C may be 能够错开，这样只用2间会议室就好了。  
那我们反过来想，每个会议先给一个房间，如果有新的 meeting start 在之前的 meeting 的 end 之后的，那可以利用之前的 meeting，房间数量减少一个。  
那这个就需要把每个会议的起始时间分别排个序了，还需要记录之前共用会议室的那个会议，记为 pre。  

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int[] start = new int[intervals.length];
        int[] end = new int[intervals.length];
        for(int i = 0; i < intervals.length; i++) {
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }
        Arrays.sort(start);
        Arrays.sort(end);
        
        int room = intervals.length;
        int pre = 0;
        for(int i = 0; i < intervals.length; i++) {
            if(start[i] >= end[pre]) {
                room--;
                pre++;
            }
        }
        return room;
    }
}
```

