---
tags:
    - Array
    - Two Pointers
    - Sorting
    - Top Interviews
---

# [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be *stored inside the array* `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.



**Example 1:**

```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```

**Example 2:**

```
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
```

**Example 3:**

```
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
```



**Soluton:**

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        if (n == 0){
            return;
        }

        int p2 = n - 1;
        int p1 = m - 1;

        for (int i = nums1.length - 1; i >= 0; i--){
            if (p2 < 0){
                return;
            }

            if (p1 < 0){
                nums1[i] = nums2[p2];
                p2--;
                continue;
            }

            int curP1 = nums1[p1];
            int curP2 = nums2[p2];

            if (curP1 >= curP2){
                nums1[i] = curP1;
                p1--;
            }else{
                nums1[i] = curP2;
                p2--;
            }
        }
    }
}
```



```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        if (n == 0){
            return;
        }

        int p2 = nums2.length -1;
        int p1 = m - 1;
        /*
         [1,2,3,0,0,0], 
                3  5  6
                    i
             p1      

        [2,5,6]
             p2

        */
        for (int i = nums1.length -1; i >= 0; i--){
            if (p2 < 0){
                return;
            }

            if (p1 < 0){
                nums1[i] = nums2[p2];
                p2--;
                continue;
            }

            if ( nums1[p1] < nums2[p2] ){
                nums1[i] = nums2[p2];
                p2--;
            }else {
                nums1[i] = nums1[p1];
                p1--;
            }
        }
    }
}
```



```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // base case
        if (m == 0 && n == 0){
            return;
        }
        if (m==0 && n !=0){
            for (int i = 0; i < n; i++){
                nums1[i] = nums2[i];
            }
            return;
        }

        if (m != 0 && n == 0){
            return;
        }

        // [1 2, 3, 0, 0,0 ]
        //       i
        //               k
        // [2, 5, 6]
        //        j
        
        int i = m -1;               // i 代表了 数量
        int j = n -1;               // j 也是数量
        int k = nums1.length -1;        // 三个指针 不要搞错了
        while(i >=0 && j >=0){
            if (nums1[i] < nums2[j]){
                nums1[k] = nums2[j];
                j--;
                k--;
            }else{
                nums1[k] = nums1[i];
                if (k != i){
                    nums1[i] = 0;
                }
                i--;
                k--;
            }
        }

        while(j >= 0){
            nums1[k] = nums2[j];
            k--;
            j--;
        }
    }
}

// TC: O(n)
// SC: O(1)
```



```java
/* Leetcode 88 Merge Sorted Array
 You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, 
 representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1.
To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should
be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

 

Example 1:

Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
Example 2:

Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
Example 3:

Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
*/
import java.util.*;
public class MergeSortedArray88{
	public static void main(String[] args){
		int[] nums1 = new int[]{1,2,3,0,0,0};
		int[] nums2 = new int[]{2,5,6};
		int m = 3;
		int n = 3;

		int[] a2 = new int[]{1};
		int[] b2 = new int[]{};
		int a = 1;
		int b = 0;

		int[] a3 = new int[]{0};
		int[] b3 = new int[]{1};
		int x = 0;
		int y = 1;
		
		System.out.println(Arrays.toString(nums1));
		System.out.println(Arrays.toString(a2));
		System.out.println(Arrays.toString(a3));
	
		Merge(nums1, m, nums2, n);
		Merge(a2, a, b2, b);
		Merge(a3,x,b3,y);
		System.out.println(Arrays.toString(nums1));
		System.out.println(Arrays.toString(a2));
		System.out.println(Arrays.toString(a3));


	}


	public static void Merge(int[] nums1, int m, int[] nums2, int n){
		/*
		 *1 2 3 0 0 0 
		      p1    i

		        2 5 6
			    p2
	 	*/

		int p1 = m-1;
		int p2 = n-1;

		for (int i = m + n -1; i >=0 ; i--){
			if (p2 < 0){
				break;
			}

			if (nums1[i] < nums2[p2]){
				nums1[i] = nums2[p2];
				p2--;
			}else{ // nums1[i] >= nums[p2]
				nums1[i] = nums1[p1];
				p1--;
			}
		}
					

	}

	/*
	 * Time complexity: O(n)
	 * Space: O(1)
	 */

}

```

