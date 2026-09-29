---
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
    - Top Interview 150
---

# [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

Given the `root` of a binary tree, *check whether it is a mirror of itself* (i.e., symmetric around its center).

 

**Example 1:**

![img](./101 Symmetric Tree.assets/symtree1.jpg)

```
Input: root = [1,2,2,3,4,4,3]
Output: true
```

**Example 2:**

![img](./101 Symmetric Tree.assets/symtree2.jpg)

```
Input: root = [1,2,2,null,3,null,3]
Output: false
```



Solution:

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
    public boolean isSymmetric(TreeNode root) {
        return isSymmetric(root.left, root.right);
    }

    public boolean isSymmetric(TreeNode p, TreeNode q){
        if (p == null && q == null){
            return true;
        }

        if (p == null || q == null){
            return false;
        }

        if (p.val != q.val){
            return false;
        }

        return isSymmetric(p.left, q.right) && isSymmetric(p.right, q.left);
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
    public boolean isSymmetric(TreeNode root) {
        // base case 
        if (root == null){
            return true;
        }
        
        return helper(root.left, root.right);
    }

    private static boolean helper(TreeNode left, TreeNode right){
        if (left == null && right == null){
            return true;
        }

        if (left == null && right != null){
            return false;
        }

        if (left != null && right == null){
            return false;
        }

        // left  != null && right != null
        if (left.val != right.val){
            return false;
        }

        return (helper(left.left, right.right) && helper(left.right, right.left));
    }
}


/*

    root not worry it

    left , right 

    case 1  left= null  right=null is true
    case 2  left = null right !=null is false
    case 3  left != null right = null is false
    case 4  => left && right != null 
                if left.val != right.val return false
    

          left.left, right.right   &&   left.right right.left.
     

*/
```

**Complexity Analysis**

- Time complexity : O(n). Because we traverse the entire input tree once, the total run time is O(n), where n is the total number of nodes in the tree.
- Space complexity : The number of recursive calls is bound by the height of the tree. In the worst case, the tree is linear and the height is in O(n). Therefore, space complexity due to recursive calls on the stack is O(n) in the worst case.



![Screenshot 2025-09-11 at 20.33.19](./101. Symmetric Tree/Screenshot 2025-09-11 at 20.33.19.png)

![Screenshot 2025-09-11 at 20.33.34](./101. Symmetric Tree/Screenshot 2025-09-11 at 20.33.34.png)

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
    public boolean isSymmetric(TreeNode root) {
        if (root == null){
            return true;
        }

        return isSymmetric(root.left, root.right);
    }

    private boolean isSymmetric(TreeNode p, TreeNode q){
        if (p == null && q == null){
            return true;
        }

        if (p == null || q == null){
            return false;
        }

        if (p.val != q.val){
            return false;
        }

        return isSymmetric(p.left, q.right) && isSymmetric(p.right, q.left);
    }
}

// TC: O(n)
// SC: O(n)
```

