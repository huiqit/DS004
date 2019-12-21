## Leetcode 155. Min Stack
这题是让我们在基本版的stack上再加一个getMin()的API，思路比较容易，就是再用一个stack存着min。  
具体操作就是：  
stack1: 普通stack，存着push来的数；  
stack2: 存目前stack1中的最小值.  
比如：  
我们陆续push 5, 1, 2, -1, 3 进来，那么两个stack的变化为：  
stack1: 5, 1, 2, -1, 3.  
stack2: 5, 1, -1.  
这样有个disadvantage，那就是如果有很多duplicates的话，stack2也要存很多次，有些浪费空间。改进方法我们稍后再说。  
```java
class MinStack {

    Deque<Integer> stack1;
    Deque<Integer> stack2; // store the min value 
    
    public MinStack() {
        stack1 = new ArrayDeque<>();
        stack2 = new ArrayDeque<>();
    }
    
    public void push(int x) {
        stack1.offerFirst(x);
        if(stack2.isEmpty() || x <= stack2.peekFirst()) {
            stack2.offerFirst(x);
        }
    }
    
    public void pop() {
        int a = stack1.peekFirst();
        if(a == stack2.peekFirst()) { //这里必须要分开写！！debug半天，否则两边都是object，而当数不在128以内时就不行了
            stack2.pollFirst();
        }
        stack1.pollFirst();
    }
    
    public int top() {
        return stack1.peekFirst();
    }
    
    public int getMin() {
        return stack2.peekFirst();
    }
}
```



