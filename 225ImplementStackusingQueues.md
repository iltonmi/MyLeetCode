# 225. Implement Stack using Queues

```java
class MyStack {
	/**
	*  2个队列，只有一个是存在元素的，每次插入只插入到非空的那一个队列
	*  每次pop，将非空队列中除了最后一个元素以外的元素转移到空的队列，然后返回非空队列的队尾元素
	*  top同理。
	*/
    private final Queue<Integer> inputQueue;
    private final Queue<Integer> transferQueue;


    /**
     * Initialize your data structure here.
     */
    public MyStack() {
        inputQueue = new ArrayDeque<>();
        transferQueue = new ArrayDeque<>();
    }

    /**
     * Push element x onto stack.
     */
    public void push(int x) {
        if (!inputQueue.isEmpty()) {
            inputQueue.add(x);
        } else {
            transferQueue.add(x);
        }

    }

    /**
     * Removes the element on top of the stack and returns that element.
     */
    public int pop() {
        if (!inputQueue.isEmpty()) {
            for (int i = 0, size = inputQueue.size() - 1; i < size; i++) {
                transferQueue.add(inputQueue.poll());
            }
            return inputQueue.poll();
        }
        for (int i = 0, size = transferQueue.size() - 1; i < size; i++) {
            inputQueue.add(transferQueue.poll());
        }
        return transferQueue.poll();

    }

    /**
     * Get the top element.
     */
    public int top() {
        int top = 0;
        if (!inputQueue.isEmpty()) {
            for (int i = 0, size = inputQueue.size(); i < size; i++) {
                top = inputQueue.poll();
                transferQueue.add(top);
            }
            return top;
        }
        for (int i = 0, size = transferQueue.size(); i < size; i++) {
            top = transferQueue.poll();
            inputQueue.add(top);
        }
        return top;
    }

    /**
     * Returns whether the stack is empty.
     */
    public boolean empty() {
        return inputQueue.isEmpty() && transferQueue.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

 