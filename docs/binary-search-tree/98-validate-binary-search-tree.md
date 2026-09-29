---
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Binary Tree
    - Top Interviews
---

# [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys less than the node's key.

- The right subtree of a node contains only nodes with keys **greater than** the node's key.

- Both the left and right subtrees must also be binary search trees.

 

**Example 1:**

![img](./98 Validate Binary Search Tree.assets/tree1.jpg)

```
Input: root = [2,1,3]
Output: true
```

**Example 2:**

![img](./98 Validate Binary Search Tree.assets/tree2.jpg)

```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```



**Solution:**

![Screenshot 2025-09-12 at 10.32.39](./98. Validate Binary Search Tree/Screenshot 2025-09-12 at 10.32.39.png)

1. 前序遍历: 先判断, 再递归

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private boolean isValidBST(TreeNode node, long left, long right){
        if (node == null){
            return true;
        }

        long cur = node.val;
        return left < cur && cur < right && isValidBST(node.left, left, cur) && isValidBST(node.right, cur, right);
    }
}

// TC: O(n)
// SC: O(n)
```

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isValidBST(TreeNode root) {
       return check(root, Long.MIN_VALUE, Long.MAX_VALUE); 

    }

    private boolean check(TreeNode node, long left, long right){
        if (node == null){
            return true;
        }
        long cur = node.val;

        if (left >= cur || cur >= right){
            return false;
        }

        return check(node.left, left, cur) && check(node.right, cur, right);
        
    }
}
// TC: O(n)
// SC: O(n)
```



2. 中序遍历: 大于上一个节点

   ```java
   /**
    * Definition for a binary tree node.
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode() {}
    *     TreeNode(int val) { this.val = val; }
    *     TreeNode(int val, TreeNode left, TreeNode right) {
    *         this.val = val;
    *         this.left = left;
    *         this.right = right;
    *     }
    * }
    */
   class Solution {
       long pre = Long.MIN_VALUE;
       public boolean isValidBST(TreeNode root) {
           if (root == null){
               return true;
           }
   
           if (!isValidBST(root.left) || root.val <= pre){
               return false;
           }
   
           pre = root.val;
           return isValidBST(root.right);
       }
   }
   // TC: O(n)
   // SC: O(n)
   ```

   ```java
   /**
    * Definition for a binary tree node.
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode() {}
    *     TreeNode(int val) { this.val = val; }
    *     TreeNode(int val, TreeNode left, TreeNode right) {
    *         this.val = val;
    *         this.left = left;
    *         this.right = right;
    *     }
    * }
    */
   class Solution {
       private long pre = Long.MIN_VALUE;
       public boolean isValidBST(TreeNode root) {
           if (root == null){
               return true;
           }
   
           if (isValidBST(root.left) == false){
               return false;
           }
   
           if (root.val <= pre){
               return false;
           }
   
           pre = root.val;
           return isValidBST(root.right);
       }
   }
   // TC: O(n)
   // SC: O(n)
   ```

   

3. 后序遍历: 先递归, 再判断

   ![Screenshot 2024-11-26 at 13.12.10](./98. Validate Binary Search Tree/Screenshot 2024-11-26 at 13.12.10.png)

   ```java
   /**
    * Definition for a binary tree node.
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode() {}
    *     TreeNode(int val) { this.val = val; }
    *     TreeNode(int val, TreeNode left, TreeNode right) {
    *         this.val = val;
    *         this.left = left;
    *         this.right = right;
    *     }
    * }
    */
   class Solution {
       public boolean isValidBST(TreeNode root) {
           return dfs(root)[1] != Long.MAX_VALUE;
       }
   
       private long[] dfs(TreeNode node){
           if (node == null){
               return new long[]{Long.MAX_VALUE, Long.MIN_VALUE};
           }
   
           long[] left = dfs(node.left);
           long[] right = dfs(node.right);
   
           long x = node.val;
   
           if (x <= left[1] || x >= right[0]){
               return new long[]{Long.MIN_VALUE, Long.MAX_VALUE};
           }
   
           return new long[]{Math.min(left[0], x), Math.max(right[1], x)};
       }
   }
   
   // ???? 
   ```

   ```java
   /**
    * Definition for a binary tree node.
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode() {}
    *     TreeNode(int val) { this.val = val; }
    *     TreeNode(int val, TreeNode left, TreeNode right) {
    *         this.val = val;
    *         this.left = left;
    *         this.right = right;
    *     }
    * }
    */
   class Solution {
       public boolean isValidBST(TreeNode root) {
           return f(root)[1] != Long.MAX_VALUE;
       }
   
       private long[] f(TreeNode node){
           if (node == null){
               return new long[]{Long.MAX_VALUE, Long.MIN_VALUE};
           }
   
           long[] left = f(node.left);
           long[] right = f(node.right);
           long cur = node.val;
   
           if (left[1] >= cur  ||  cur >= right[0]){
               return new long[]{Long.MIN_VALUE, Long.MAX_VALUE};
           }
   
           return new long[]{Math.min(left[0], cur), Math.max(right[1], cur)};
       }
   }
   ```
   
   

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null){
            return true;
        }

        return helper(root, Long.MIN_VALUE, Long.MAX_VALUE);
        
    }

    private static boolean helper(TreeNode root, long min, long max){
        if (root == null){
            return true;
        }

        if (root.val <= min || root.val >= max){
            return false;
        }


        return helper(root.left,min, root.val) && helper(root.right,root.val, max);

    }
}

// Long has been taken and it will work because node values are between Integer.MAX_VALUE and Integer.MIN_VALUE and Long has broader range than Integer
// TC: O(n)
// SC: O(n)

/*
Time complexity : O(N)\mathcal{O}(N)O(N) since we visit each node exactly once.
Space complexity : O(N)\mathcal{O}(N)O(N) since we keep up to the entire tree.
*/
```





#### Binary Search Tree

**==经典例题1: How to determine a binary tree is a BST?==**

​                                           10 == root

​            								/     \

​										5         15

​         							/   \       /    \

​         						 2     7     12   20         <- all leaf node's level == 3

​           					/   \

​						   null  null

Solution 0: in Order (very bad)

​			Step 1: Do Inorder traversal on the tree and store all the value in a arrayList

​			Step 2: check if array[i] < array[i+1]

Solution 1: in Order better

​			do not store all values just store one variable -> prevNode

​							for every node we traversal in ignorer:

​															  check if curValue > prevValue

Time = O(n)

所有的点都要过一遍



**Solution 2(Another way)**

[min, max] 这个subtree的值都应当落在这个range里

BST: 左子树的最大值 < root'value

​		 右子树的最小值 > root'value

​                                        10 == root $(-\infty,\infty)$

​            								/                    \

​              $(-\infty,10)$        5                                15   $(10, +\infty)$

​         							/   \                                    /    \

​         		$(-\infty,5)$ 2        7 (5, 10)      (10, 15)12   20 $(15,\infty)$        

​           					/    \

​						   null  null												

若5变为11则直接return false, 这边就不需要所有n都过一遍, 提前return false

往左走的时候, min不变, max变为当前层的rootvalue

往右走的时候, max不变, min变为当前层的rootvalue

```java
publlic boolean isBST(TreeNode root, int max, int min){
  		if (root == null) return true;
  		// 检查当前层的root 在不在范围里
  		if (root.key <= min || root.key > max){
       			return false;
      }
  		return isBST(root.left, root.key, min) && isBST(root.right, max, root.key)
}
```

Time: O(n)

Space: O(height)



1

​	\ 

​		3

​			\

​				5

这个是个binary search tree 但它不balance



==**Discussion**==

==Recursion在tree题目基本应用大致分为2类用法==

1. ==把value从上往下传递then从下往上的题目==

   ==1.1 BST判定方法==

2. ==只把value从下往上传递 (更为常见, 必须熟练掌握)==

   ==2.1. getHeight(Node root) 是经典的把Integer value从下往上传递的题目==

   ==2.2. isBalanced(Node root) 是把boolean value从下往上传递的题目==

   ==2.3. isSymmetric(Node root1, Node root2)是把boolean value从下往上传送的题目==

   ==2.4. Assign the value of each node to be the total number of nodes that belong to its left subtree (是把integer value 从下往上传递的题目)==








https://www.bilibili.com/video/BV14G411P7C1?spm_id_from=333.788.videopod.sections&vd_source=73e7d2c4251a7c9000b22d21b70f5635







