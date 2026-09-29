---
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
    - Top Interviews
---

# [637. Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/)

Given the `root` of a binary tree, return *the average value of the nodes on each level in the form of an array*. Answers within `10-5` of the actual answer will be accepted.

 

**Example 1:**

![img](./637. Average of Levels in Binary Tree/avg1-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [3.00000,14.50000,11.00000]
Explanation: The average value of nodes on level 0 is 3, on level 1 is 14.5, and on level 2 is 11.
Hence return [3, 14.5, 11].
```

**Example 2:**

![img](./637. Average of Levels in Binary Tree/avg2-tree.jpg)

```
Input: root = [3,9,20,15,7]
Output: [3.00000,14.50000,11.00000]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-231 <= Node.val <= 231 - 1`



**Solution:**

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
    public List<Double> averageOfLevels(TreeNode root) {
       List<Double> result = new ArrayList<>();
       if (root == null){
        return result;
       }

       Deque<TreeNode> queue = new ArrayDeque<>();
       queue.offerLast(root);

       while(!queue.isEmpty()){
        int size = queue.size();
        double curSum = 0;
        for (int i = 0; i < size; i++){
            TreeNode cur = queue.pollFirst();
            curSum = curSum + cur.val;

            if (cur.left != null){
                queue.offerLast(cur.left);
            }

            if (cur.right != null){
                queue.offerLast(cur.right);
            }
        }

        result.add(((double) curSum / (double) size));
       }

       return result;

    }
}

// TC: O(n)
// SC: O(n)
```

