## Leetcode 394. Decode String
input中共有4种符号，数字、字母、[、]。那我们就分情况讨论就行了。  
关键是结果如何储存：用一个StringBuilder的stack，至少2层，下面一层将会是是最终结果，顶层是当前处理的这个String，如果还有中间层，意味着有嵌套型的那种组合。  
遇到数字时，要先把数字存下来，由于不一定立刻就用到这个数字（比如3[a[2[b]]里的3要在2b解锁之后才会用上），所以需要一个stack存起来，且存的是当前连在一起的这个数字，比如23[abc]存23，意味着后面这块StringBuilder会重复23次。  
遇到[时，就意味着接下来是letter了，或者是letter + 数字 + letter。  
遇到]时，就意味着处理完这一小块了，可以把这块StringBuilder和上一层的合并，遇到一个]合并一个。  
遇到字母，直接加就是。  
2个bug：  
- strStack一开始要加个空string进去，否则npe。如果存String就不需要了；
- 查数字是要包括0，虽然首位不会是0，但是其他位可以

```java
// Runtime: 0 ms, faster than 100.00% of Java online submissions for Decode String.
// Memory Usage: 34 MB, less than 100.00% of Java online submissions for Decode String.

class Solution {
  public String decodeString(String s) {
    Deque<StringBuilder> strStack = new ArrayDeque<>();
    Deque<Integer> intStack = new ArrayDeque<>();
    int i = 0;
    char[] array = s.toCharArray();
    strStack.offerFirst(new StringBuilder()); //一开始要加个空string进去，否则npe。如果存String就不需要了
    
    while(i < s.length()) {
      char c = array[i];
      if(c >= '0' && c <= '9') { // 还必须要等于0，比如100
        int count = 0;
        while(array[i] >= '0' && array[i] <= '9') {
          count = 10 * count + array[i] - '0';
          i++;
        }
        intStack.offerFirst(count);
      } else if (c == '[') {
        strStack.offerFirst(new StringBuilder());
        i++;
      } else if (c == ']') {
        String curr = strStack.pollFirst().toString();
        int count = intStack.pollFirst();
        for(int j = 0; j < count; j++) {
          strStack.peekFirst().append(curr);
        }
        i++;
      } else {
        strStack.peekFirst().append(c);
        i++;
      }
    }
    return strStack.pollFirst().toString();
  }
}
      

```
