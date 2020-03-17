#  232. Implement Queue using Stacks 

```java
/**
* 将一组元素顺序压入临时栈中，再将临时栈中的元素逐个弹出并压入新的栈
* 新的栈弹出元素的顺序，相当于队列中元素出队的顺序
*/
class MyQueue {
    
    //当需要peek或者pop时，先检查这个栈是否为空。
    //若是，则将临时栈中元素逐个弹出并压入这个栈
    //最后，peek或pop这个栈的栈顶元素
    Stack<Integer> stack = new Stack<>();
    
    //所有push操作都将元素压入临时栈
    Stack<Integer> tempStack = new Stack<>();

    /** Initialize your data structure here. */
    public MyQueue() {
        
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        this.tempStack.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        fillStackIfEmpty();
        return this.stack.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        fillStackIfEmpty();
        return this.stack.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return this.stack.isEmpty() && this.tempStack.isEmpty();
    }
    
    private void fillStackIfEmpty(){
        if(!this.stack.isEmpty()){
            return;
        }
        while(!this.tempStack.isEmpty()){
            this.stack.push(this.tempStack.pop());
        }
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

