## Leetcode146. LRU cache
这是一道相当高频的题了  
LRU means least recently used, it is a cache eviction policies. 
cache就是缓存，为了加快访问速度的，但是它有一定的容量限制，所以当达到容量上限时，就把那些很久不用的信息踢掉。  
具体的operations就是get & put，往里面读/写信息  
1. get info，这里我们需要一个能够快速读信息的data structure --> HashMap O(1) 时间可以做到  
在读取之后，这条信息就是最新被访问的了，那还需要调整一下它的顺序，然而hashMap是无序的，所以我们还需要一个有序的数据结构来记录这个顺序。而这里我们是根据访问时间给它的顺序，所以LinkedList可以。
但是这里有个问题，如何快速定位到这个key的在LinkedList的位置呢？我们肯定不想一个个找一遍。。而HashMap可以O(1)拿到key，那我们把它的value设成相应key的ListNode的reference就好了。  
那我们就有了基本思路：
hashMap: <int key, ListNode node> 
LinkedList: <key, value> --> <key, value> --> ...
              head（新用的）                   tail (least recently used)
再看put  
2. 当新加cache的内容时：  
首先，要看cache里有没有这个key，
  * 如果有，那就update value并且调整顺序，变成最新used，也就是remove node + append at Head;
  * 如果没有，那就看cache里还有没有足够的空间，
    * 如果有空，那就直接加进去就好了，即appendHead;
    * 如果没空了，那就得删掉least recently used 即 删除tail，再把新的加进来 即 appendHead。
相信已经说的很清楚了，看代码吧：



