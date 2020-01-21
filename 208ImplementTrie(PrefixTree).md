#  208. Implement Trie (Prefix Tree) 

  https://leetcode.com/articles/implement-trie-prefix-tree/ 

 Trie (we pronounce "try") or prefix tree is a tree data structure, which is used for retrieval of a key in a dataset of strings.  There are various applications of this very efficient data structure such as : 

	1. AutoComplete
 	2. SpellChecker
 	3. IP Routing(Longest Prefix Matching)

### 和其他数据结构的比较

平衡树和散列表等等的数据结构都可以用来在字符串集合中搜索单词，那为什么还需要前缀树呢？

**首先，和散列表相比**，能够在O(1)的时间内查找一个key。

缺点：

1. 在以下2种操作中非常低效：
   1. 查找有相同前缀的所有key
   2. 按照词典序( lexicographical order)枚举所有字符串
2. 当散列表的大小增加，散列冲突增加，查找key的时间复杂度可能退化( deteriorate )到O(N)
3. 存储具有相同前缀的key时，Trie使用更少的空间。

**然后，和平衡树相比**，能够在O(MlogN)的时间内查找一个key。M是key的长度。

缺点：

1. 散列表只需要O(M)的时间查找一个key



### Trie Node前缀树节点

前缀树的2个成员变量：

1.  Maximum of *R* links to its children, where each link corresponds to one of *R* character values from dataset alphabet. 
2.  Boolean field which specifies whether the node corresponds to the end of the key, or is just a key prefix.

实现：

```java
 class TrieNode {

	// R links to node children
    private TrieNode[] links;

    private final int R = 26;

    private boolean isEnd;

    public TrieNode() {
        links = new TrieNode[R];
    }

    public boolean containsKey(char ch) {
        return links[ch -'a'] != null;
    }
    public TrieNode get(char ch) {
        return links[ch -'a'];
    }
    public void put(char ch, TrieNode node) {
        links[ch -'a'] = node;
    }
    public void setEnd() {
        isEnd = true;
    }
    public boolean isEnd() {
        return isEnd;
    }
}
```



### Insertion

2 cases:

1.  A link exists. Then we move down to continues with searching for the next key character. 
2.  A link does not exist. Then we create a new node and link it with the parent's link matching the current key character.   We repeat this step until we encounter the last character of the key, then we mark the current node as an end node and the algorithm finishes. 

时间O(M)；空间O(M)，最坏情况下需要创建全部M个新节点。

```java
class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
            char currentChar = word.charAt(i);
            if (!node.containsKey(currentChar)) {
                node.put(currentChar, new TrieNode());
            }
            node = node.get(currentChar);
        }
        node.setEnd();
    }
}
```



### Search

2 cases:

1.  A link exist. We move to search for the next key character. 
2.  A link does not exist. If there are no available key characters and current node is marked as `isEnd` we return true. 

时间O(M)，空间O(1)

```java
class Trie {
    ...

    // search a prefix or whole key in trie and
    // returns the node where search ends
    private TrieNode searchPrefix(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
           char curLetter = word.charAt(i);
           if (node.containsKey(curLetter)) {
               node = node.get(curLetter);
           } else {
               return null;
           }
        }
        return node;
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
       TrieNode node = searchPrefix(word);
       return node != null && node.isEnd();
    }
}
```



### Search for a key prefix

时间O(M)，空间O(1)

```java
class Trie {
    ...

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        TrieNode node = searchPrefix(prefix);
        return node != null;
    }
}
```





### 题解

```java
class Trie {
    
    private TrieNode root;

    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        if(word == null) {
            return;
        }
        TrieNode cur = root;
        for(int i = 0; i < word.length(); i++) {
            char curChar = word.charAt(i);
            if(!cur.containsKey(curChar)) {
                cur.put(curChar, new TrieNode());
            }
            cur = cur.get(curChar);
        }
        cur.setEnd();
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode last = searchPrefix(word);
        return last != null && last.isEnd();
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        return searchPrefix(prefix) != null;
    }
    
    private TrieNode searchPrefix(String word) {
        TrieNode cur = root;
        for(int i = 0; i < word.length(); i++) {
            char curChar = word.charAt(i);
            if(!cur.containsKey(curChar)) {
                return null;
            }
            cur = cur.get(curChar);
        }
        return cur;
    }
    
    private static class TrieNode {
        
        private TrieNode[] links;
        
        private final int R = 26;
        
        private boolean end;
        
        public TrieNode() {
            links = new TrieNode[R];
        }
        
        public boolean containsKey(char ch) {
            return links[ch - 'a'] != null;
        }
        
        public TrieNode get(char ch) {
            return links[ch - 'a'];
        }
        
        public void put(char ch, TrieNode node) {
            links[ch - 'a'] = node;
        }
        
        public void setEnd() {
            end = true;
        }
        
        public boolean isEnd() {
            return end;
        }
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```

