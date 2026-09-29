---
tags:
    - String
    - Depth-First Search
    - Design
    - Trie
    - Top Interviews
---

# [211. Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:

- `WordDictionary()` Initializes the object.
- `void addWord(word)` Adds `word` to the data structure, it can be matched later.
- `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.

 

**Example:**

```
Input
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
Output
[null,null,null,null,false,true,true,true]

Explanation
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True
```

 

**Constraints:**

- `1 <= word.length <= 25`
- `word` in `addWord` consists of lowercase English letters.
- `word` in `search` consist of `'.'` or lowercase English letters.
- There will be at most `2` dots in `word` for `search` queries.
- At most `104` calls will be made to `addWord` and `search`.



**Solution:**

```java
class WordDictionary {
    private static class TrieNode{
        TrieNode[] children = new TrieNode[26];
        boolean isEnd = false;
    }

    private final TrieNode root;

    public WordDictionary() {
        root = new TrieNode();
    }
    
    public void addWord(String word) {
        TrieNode cur = root;
        for (int i = 0; i < word.length(); i++){
            TrieNode next = cur.children[word.charAt(i) - 'a'];

            if (next == null){
                next = new TrieNode();
                cur.children[word.charAt(i) - 'a'] = next;
            }
            cur = next;
        }

        cur.isEnd = true;
    }

    public boolean search(String word){
        return dfs(word, 0, root);
    }
    
    public boolean dfs(String word, int index, TrieNode cur) {

        if (index == word.length()){
            return cur.isEnd;
        }

        char ch = word.charAt(index);

        if (ch == '.'){
            for (int i = 0; i < 26; i++){
                TrieNode next = cur.children[i];
                if (next != null && dfs(word, index + 1, next)){
                    return true;
                }
            }
            return false;
        }

        TrieNode next = cur.children[ch - 'a'];
        if (next == null){
            return false;
        }

        return dfs(word, index + 1, next);

    }

}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```



```java
class WordDictionary {
    static class Trie{
        static class TrieNode{
            Map<Character, TrieNode> children = new HashMap<>();
            boolean isWord = false;
        }
        TrieNode root;

        public Trie(){
            root = new TrieNode();
        }

        public void insert(String word) {
            TrieNode cur = root;

            for (char ch : word.toCharArray()){
                if (cur.children.containsKey(ch)){
                    cur = cur.children.get(ch);
                }else{
                    cur.children.put(ch, new TrieNode());
                    cur = cur.children.get(ch);
                }
            }

            cur.isWord = true;
        }

        public boolean search(String word){
            TrieNode cur = root;
            return search(word, cur);
        }
        public boolean search(String word, TrieNode cur) {
            for (int i = 0; i < word.length(); i++){
                char ch = word.charAt(i);
                if (!cur.children.containsKey(ch)){
                    if (ch == '.'){
                        for (Map.Entry<Character, TrieNode> e : cur.children.entrySet()){
                            TrieNode child = e.getValue();
                            if (search(word.substring(i + 1), child) == true){
                                return true;
                            }
                        }
                    }
                return false;
            }else{
                cur = cur.children.get(ch);
            }
        }

            return cur.isWord;
        }
    }


    Trie trie;


    public WordDictionary() {
        trie = new Trie();
    }
    
    public void addWord(String word) {
        trie.insert(word);
    }

    public boolean search(String word) {
        return trie.search(word);
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */


// TC: O(n^2)
// SC: O(n)

```



