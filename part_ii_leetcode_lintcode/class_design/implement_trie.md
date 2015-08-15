# Implement Trie (Prefix Tree)

> Implement a trie with insert, search, and startsWith methods.
>
> Note:
>
> You may assume that all inputs are consist of lowercase letters a-z.

### Analysis
[definition of trie](http://www.geeksforgeeks.org/trie-insert-and-search/)

### Code
#### Java
```java
class TrieNode {
  // Initialize your data structure here.
  private char val;
  private TrieNode[] children;
  private boolean isLeaf;
  
  public TrieNode(char val) {
    this.val = val;
    this.children = new TrieNode[26];
  }

  public char getValue() {
    return val;
  }

  public void addChild(char val) {
    int index = getIndex(val);
    if (children[index] == null) {
      children[index] = new TrieNode(val);
    }
  }

  public TrieNode getChild(char val) {
    int index = getIndex(val);
    return children[index];
  }

  public void setLeaf() {
    isLeaf = true;
  }

  public boolean isLeaf() {
    return isLeaf;
  }

  private int getIndex(char val) {
    return val - 'a';
  }
}

public class Trie {
  private TrieNode root;

  public Trie() {
      root = new TrieNode('a');
  }

  // Inserts a word into the trie.
  public void insert(String word) {
    if (word == null) {
      return;
    }
    TrieNode cur = root;
    for (char c : word.toCharArray()) {
      cur.addChild(c);
      cur = cur.getChild(c);
    }
    cur.setLeaf();
  }

  // Returns if the word is in the trie.
  public boolean search(String word) {
    TrieNode lastNode = searchHelper(word);
    return lastNode != null && lastNode.isLeaf();
  }

  // Returns if there is any word in the trie
  // that starts with the given prefix.
  public boolean startsWith(String prefix) {
    return searchHelper(prefix) != null;
  }

  private TrieNode searchHelper(String word) {
    if (word == null) {
      return null;
    }
    TrieNode cur = root;
    for (char c : word.toCharArray()) {
      cur = cur.getChild(c);
      if (cur == null) {
        break;
      }
    }
    return cur;
  }
}

// Your Trie object will be instantiated and called as such:
// Trie trie = new Trie();
// trie.insert("somestring");
// trie.search("key");
```

