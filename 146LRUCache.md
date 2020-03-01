#  146. LRU Cache 

1. 使用HashMap和双向链表，操作时间复杂度O(1)。

   使用双向链表，从头到尾优先度递增，即头节点为最近最少使用（LRU）的节点，双向链表插入删除节点的时间复杂度都是O(1)。

   通过key获取value，因此需要使用HashMap保存key到节点的映射。

   当移除指定节点时，需要情况keyNodeMap中的entry，需要根据节点查询对应的key，因此还需要另一个HashMap保存节点到key的映射。
   
   ```java
   /**
    * Your LRUCache object will be instantiated and called as such:
    * LRUCache obj = new LRUCache(capacity);
    * int param_1 = obj.get(key);
    * obj.put(key,value);
    */
   class LRUCache {
       
       private MyLRUCache<Integer, Integer> innerCache;
   
       public LRUCache(int capacity) {
           this.innerCache = new MyLRUCache<>(capacity);
       }
       
       public int get(int key) {
           Integer res = innerCache.get(key);
           return res == null ? -1 : res;
       }
       
       public void put(int key, int value) {
           innerCache.put(key, value);
       }
   
   }
   
   class MyLRUCache<K, V> {
       private HashMap<K, Node<V>> keyNodeMap;
       private HashMap<Node<V>, K> nodeKeyMap;
       private NodeDoublyLinkedList<V> list;
       private int capacity;
       
       public MyLRUCache(int capacity) {
           this.capacity = capacity;
           this.keyNodeMap = new HashMap<>();
           this.nodeKeyMap = new HashMap<>();
           this.list = new NodeDoublyLinkedList<>();
       }
       
       public V get(K key) {
           if(!keyNodeMap.containsKey(key)) {
               return null;
           }
           Node<V> res = keyNodeMap.get(key);
           list.moveNodeToTail(res);
           return res.value;
       }
       
       public void put(K key, V value) {
           if(keyNodeMap.containsKey(key)) {
               Node<V> node = keyNodeMap.get(key);
               node.value = value;
               list.moveNodeToTail(node);
           } else {
               Node<V> node = new Node<>(value);
               keyNodeMap.put(key, node);
               nodeKeyMap.put(node, key);
               list.addNode(node);
               if(keyNodeMap.size() == capacity + 1) {
                   removeLeastRecentlyUsedCache();
               }
           }
       } 
       
       private void removeLeastRecentlyUsedCache() {
           Node<V> removedNode = list.removeHead();
           K removedKey = nodeKeyMap.get(removedNode);
           keyNodeMap.remove(removedKey);
           nodeKeyMap.remove(removedNode);
       }
   }
   
   /**
   *   优先级从头到尾递增
   **/
   class NodeDoublyLinkedList<E> {
       private Node<E> head;
       private Node<E> tail;
       
       public NodeDoublyLinkedList() {
           this.head = new Node<>();
           this.tail = new Node<>();
           head.next = tail;
           tail.pre = head;
       }
       
       public void addNode(Node<E> newNode) {
           if(newNode == null) {
               return;
           }
           newNode.next = tail;
           newNode.pre = tail.pre;
           tail.pre.next = newNode;
           tail.pre = newNode;
       }
       
       public void moveNodeToTail(Node<E> node) {
           node.pre.next = node.next;
           node.next.pre = node.pre;
           addNode(node);
       }
       
       public Node<E> removeHead() {
           if(head.next == tail) {
               return null;
           }
           
           Node<E> node = head.next;
           node.next.pre = head;
           head.next = node.next;
           node.pre = null;
           node.next = null;
           
           return node;
       }
   }
   
   class Node<T> {
       public T value;
       public Node<T> pre;
       public Node<T> next;
       
       public Node() {
           
       }
       
       public Node(T value) {
           this.value = value;
       }
}
   ```
   
   