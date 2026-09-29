---
tags:
    - Hash Table
    - String
    - Design
    - Trie
    - Top Interviews
---

# [208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

A [**trie**](https://en.wikipedia.org/wiki/Trie) (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

- `Trie()` Initializes the trie object.
- `void insert(String word)` Inserts the string `word` into the trie.
- `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
- `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

**Example 1:**

```
Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
```

**Constraints:**

- `1 <= word.length, prefix.length <= 2000`
- `word` and `prefix` consist only of lowercase English letters.
- At most `3 * 104` calls **in total** will be made to `insert`, `search`, and `startsWith`.

**Solution:**

**Idea:** 

>  "If we both have walked the same path, then our hearts are connected."

Assume the string contains only two characters: **a** and **b**.

When traversing the string from left to right, treat **a** as a step to the left and **b** as a step to the right. How can we record this path? How can we distinguish different paths? We can use a **binary tree** to record them.

* **Insert:** For example, inserting the string `aab` is equivalent to generating a path with moves “left–left–right.” Mark the final node as an end node. Then inserting `aabb` generates the path “left–left–right–right.” Mark its final node as an end node as well.
* **Search:** For example, searching for `aab` means checking whether there exists a path “left–left–right” in the binary tree, and whether the final node is an end node.
* **startsWith:** For example, searching for prefix `aa` means checking whether there exists a path “left–left” in the binary tree, without any other condition.

![lc208.png](./208. Implement Trie (Prefix Tree)/1732008195-ouktYd-lc208.png)

When generalized to 26 letters, it becomes a **26-ary tree**. Each node in the 26-ary tree can use either a **hash map** or an **array of length 26** to store its child nodes.

**Algorithm**

* **Initialization:** Create a 26-ary tree with only one root node `root`. Each node in the tree has: an array `son` of length 26 for its child nodes, and a boolean `end` indicating whether it’s an end node.

* **Insert:**
  1. Traverse the string `word`, using a variable `cur` to represent the current node, starting from `root`.
  2. If `word[i]` is not one of `cur`’s children, create a new node `node` as `cur`’s child. If `word[i] = 'a'`, store `node` in `cur.son[0]`; if `word[i] = 'b'`, store it in `cur.son[1]`, and so on.
  3. Then update `cur` to point to that child node. 
  4. After the traversal, mark `cur.end = true`.

* **search** and **startsWith** can both reuse a helper function **find**: 
  1. Traverse the string `word`, using a variable `cur` starting from `root`.
  2.  If `word[i]` is not a child of `cur`, return `0`. (`search` and `startsWith` will interpret `0` as `false`.)
  3. Update `cur` to the corresponding child node. 
  4. After traversal: If `cur.end` is `false`, return `1`; If `cur.end` is `true`, return `2`. Then:`search` returns `true` if it receives `2`, otherwise `false`;`startsWith` returns `true` if it receives a non-zero value, otherwise `false`.

```java
class Trie {
    private static class TrieNode{
        TrieNode[] children = new TrieNode[26];
        boolean end = false;
    }
    private final TrieNode root;

    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode cur = root;

        for (int i = 0; i < word.length(); i++){
            TrieNode next = cur.children[word.charAt(i) - 'a'];
            if (next == null){
                next = new TrieNode();
                cur.children[word.charAt(i) - 'a'] = next;
            }
            cur = next;
        }
        cur.end = true;
    }
    
    public boolean search(String word) {
        return find(word) == 2;
    }
    
    public boolean startsWith(String prefix) {
        return find(prefix) != 0;
    }

    private int find(String word){
        TrieNode cur = root;

        for (int i = 0; i < word.length(); i++){
            TrieNode next = cur.children[word.charAt(i) - 'a'];

            if (next == null){
                return 0;
            }
            cur = next;
        }
        
        if (cur.end == true){
            return 2;
        }else{
            return 1;
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


// TC: 初始化为 O(1)，insert 为 O(n∣Σ∣)，其余为 O(n)，其中 n 是 word 的长度，∣Σ∣=26 是字符集合的大小。注意创建一个节点需要 O(∣Σ∣) 的时间（如果用的是数组）。
// SC: 空间复杂度：O(qn∣Σ∣)。其中 q 是 insert 的调用次数。
```



```java
class Trie {
    class TrieNode{
        TrieNode[] children = new TrieNode[26];
        boolean isWord = false;
    }

    TrieNode root;

    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode cur = root;
        for (int i = 0; i < word.length(); i++){
            TrieNode next = cur.children[word.charAt(i) - 'a'];
            if (next == null){
                next = new TrieNode();
                cur.children[word.charAt(i) - 'a'] = next;
            }
            cur = next;
        }

        cur.isWord = true;
    }
    
    public boolean search(String word) {
        TrieNode cur = root;

        for (int i = 0; i < word.length(); i++){
            TrieNode next = cur.children[word.charAt(i) - 'a'];
            if (next == null){
                return false;
            }

            cur = next;
        }

        if (cur.isWord == true){
            return true;
        }else {
            return false;
        }
    }
    
    public boolean startsWith(String prefix) {
        TrieNode cur = root;
        for (int i = 0; i < prefix.length(); i++){
            TrieNode next = cur.children[prefix.charAt(i) - 'a'];
            if (next == null){
                return false;
            }
            cur = next;
        }

        return true;

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



Convert Methods to Instance Methods: 

```java
package data_structures.trie;

import java.util.HashMap;
import java.util.Map;

/**
 * @Author: eve
 * @Date: 2025-01-03 23:39
 */
public class Trie {
    private final TrieNode root;
    private static class TrieNode{
        Map<Character, TrieNode> children = new HashMap<>();
        boolean isWord;
    }
  // TrieNode 本身是一个通用节点结构，只存储子节点和是否为单词结尾的信息。它的行为完全独立于 Trie 的实例
  // 如果 TrieNode 是非 static 的，节点与 Trie 的实例会产生隐式的耦合
    public Trie(){
        root = new TrieNode();
    }

    public void insert(String word){ // L = word.length
        TrieNode cur = root;
        for (int i = 0; i < word.length(); i++){
            char ch = word.charAt(i);
            if (!cur.children.containsKey(ch)) {
                cur.children.put(ch, new TrieNode());
            }
            cur = cur.children.get(ch);
        }

        cur.isWord = true;
    }
  
  // TC: O(L)

    public boolean search(String word){
        TrieNode cur = root;

        for (int i = 0; i < word.length(); i++){
            char ch = word.charAt(i);
            if (!cur.children.containsKey(ch)){
                return false;
            }else{
                cur = cur.children.get(ch);
            }
        }

        return cur.isWord;

    }
  
  // TC: O(L)

    public boolean startsWith(String prefix){
        TrieNode cur = root;

        for (int i = 0; i < prefix.length(); i++){
            char ch = prefix.charAt(i);
            if (cur.children.containsKey(ch)){
                cur = cur.children.get(ch);
            }else{
                return false;
            }
        }

        return true;
    }
		// TC: O(P)
}


// n words
// TC: O(L)
// SC: O(N*L)
```

`Main.java`:

```java
package data_structures.trie;

/**
 * @Author: eve
 * @Date: 2025-01-04 00:08
 */
public class Main {
    public static void main(String[] args) {
        Trie trie = new Trie();

        // insert
        trie.insert("apple");
        trie.insert("app");

        // search
        System.out.println("Search 'apple': " + trie.search("apple")); // true
        System.out.println("Search 'app': " + trie.search("app"));     // true
        System.out.println("Search 'appl': " + trie.search("appl"));   // false

        // prefix
        System.out.println("Starts with 'ap': " + trie.startsWith("ap")); // true
        System.out.println("Starts with 'appl': " + trie.startsWith("appl")); // true
        System.out.println("Starts with 'bat': " + trie.startsWith("bat")); // false
    }
}

```

output:

```
Search 'apple': true
Search 'app': true
Search 'appl': false
Starts with 'ap': true
Starts with 'appl': true
Starts with 'bat': false
```



`static` : 

* 修饰的变量方法的属于类本身, 而不是某个对象.
* 不管你创建了多少个`Trie`对象, 所有对象都共享同一个`static` 成员

Trie的逻辑设计:

* 每个`Trie`应该有自己独立的数据结构来存储单词(TrieNode)
* 如果用`static`, 那么所有的`Trie`实例都会共用同一个`root`节点. 
  * 插入到一个 `Trie` 中的数据会自动影响所有其他的 `Trie` 实例
  * 这不是我们希望的，因为我们通常希望每个 `Trie` 都可以独立操作自己的数据。

* 去掉 `static` 后，`Trie` 类中的 `root` 和方法都属于某个具体的 `Trie` 实例
* 每个 `Trie` 实例有自己的 `root`，可以独立存储单词和前缀





Use Class Name for Static Methods (Not recommend)

```java
package data_structures.trie;

import java.util.HashMap;
import java.util.Map;

/**
 * @Author: eve
 * @Date: 2025-01-04 00:31
 */
public class Trie2NotRecommend {
    private static TrieNode root = new TrieNode(); // Instance-specific root
  // private: 表示完全封装, 只有在同一个类中可以访问
  // 强制其他类通过特定方法（如 getter 和 setter）访问数据
    public static class TrieNode{
        Map<Character, TrieNode> children = new HashMap<>();
        boolean isWord;
    }

    public Trie2NotRecommend(){
    }

    public static void insert(String word){
        TrieNode cur = root;
        for (int i = 0; i < word.length(); i++){
            char ch = word.charAt(i);
            if (!cur.children.containsKey(ch)) {
                cur.children.put(ch, new TrieNode());
            }
            cur = cur.children.get(ch);
        }

        cur.isWord = true;
    }

    public static boolean search(String word){ // static, main 中才可以调用
        TrieNode cur = root;

        for (int i = 0; i < word.length(); i++){
            char ch = word.charAt(i);
            if (!cur.children.containsKey(ch)){
                return false;
            }else{
                cur = cur.children.get(ch);
            }
        }

        return cur.isWord;

    }

    public static boolean startsWith(String prefix){
        TrieNode cur = root;

        for (int i = 0; i < prefix.length(); i++){
            char ch = prefix.charAt(i);
            if (cur.children.containsKey(ch)){
                cur = cur.children.get(ch);
            }else{
                return false;
            }
        }

        return true;
    }

}
```

`Main.java`:

```java
package data_structures.trie;

/**
 * @Author: eve
 * @Date: 2025-01-04 00:08
 */
public class Main {
    public static void main(String[] args) {
        // Insert words into the Trie
        Trie2NotRecommend.insert("apple");
        Trie2NotRecommend.insert("app");

        // Search for words
        System.out.println("Search 'apple': " + Trie2NotRecommend.search("apple")); // true
        System.out.println("Search 'app': " + Trie2NotRecommend.search("app"));     // true
        System.out.println("Search 'appl': " + Trie2NotRecommend.search("appl"));   // false

        // Check for prefixes
        System.out.println("Starts with 'ap': " + Trie2NotRecommend.startsWith("ap")); // true
        System.out.println("Starts with 'appl': " + Trie2NotRecommend.startsWith("appl")); // true
        System.out.println("Starts with 'bat': " + Trie2NotRecommend.startsWith("bat")); // false
    }
}

```

```
Search 'apple': true
Search 'app': true
Search 'appl': false
Starts with 'ap': true
Starts with 'appl': true
Starts with 'bat': false
```



Problem (even it can pass leetcode):

1. `root` 被声明为 `static`，这意味着它是全局共享的，属于类 `Trie2NotRecommend` 本身，而不是某个具体的实例。
2. 当多个 `Trie2NotRecommend` 对象被创建时，它们都会共享同一个 `root`。如果一个对象修改了 `root` 的内容，所有对象都会受到影响。

Why:

- `Trie` 的设计本质上是一个独立的数据结构，每个实例应该有自己的独立 `root` 和 `children`。
- 如果共享同一个 `root`，会导致数据冲突，无法区分不同 `Trie` 实例中的数据。

设计违背面向对象原则

- **问题**:
  - `Trie2NotRecommend` 的设计是一个类级别（`static`）的工具，而不是一个实例级别的数据结构。
  - 这种设计会导致数据结构在逻辑上无法独立运作，不符合面向对象设计的原则。
- **为什么不推荐**:
  - 数据结构通常需要实例级别的封装和操作，而 `static` 的全局共享违背了封装的原则。
