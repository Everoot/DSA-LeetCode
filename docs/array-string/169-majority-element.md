---
tags:
    - Array
    - Hash Table
    - Divide and Conquer
    - Sorting
    - Counting
    - Top Interviews
---

# [169. Majority Element](https://leetcode.com/problems/majority-element/)

Given an array `nums` of size `n`, return *the majority element*.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

**Example 1:**

```
Input: nums = [3,2,3]
Output: 3
```

**Example 2:**

```
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```



**Solution:**

```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length/2];
    }
}

// TC: O(nlogn)
// SC: O(n) or O(1)
```



**Boyer-Moore Voting Algorithm**

```java
class Solution {
    public int majorityElement(int[] nums) {
        int candidate = nums[0]; // 3
        int count = 1;
        for (int i = 1; i < nums.length; i++){
            if (count == 0){
                count++;
                candidate = nums[i];
            }else if (candidate == nums[i]){
                count++;
            }else{
                count--;
            }
        }
        return candidate;
    }
}
//  Boyer-Moore Voting Algorithm

// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public int majorityElement(int[] nums) {
        int majCount = 0;
        int majNum = 0;
        int n = nums.length; 

        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++){
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);

            if (majCount <= map.get(nums[i])){
                majCount = map.get(nums[i]);
                majNum = nums[i];
            }

            if (majCount > n/2){
                return majNum;
            }
        }
        return -1;
    }
}
```

**Problem:** Find the strict majority element in `nums`. A strict majority element is one whose number of occurrences is greater than the total occurrences of all other elements combined.

The problem guarantees that such a strict majority element always exists.

Try to design an algorithm that runs in **O(n)** time (single pass) and uses only **O(1)** extra space.

**Intuition (Martial Arts Tournament Analogy)**

Imagine a martial arts tournament where fighters compete on a stage. Who will remain standing at the end?

**We use an “arena challenge” analogy:**

* **Champion enters:** `nums[0]` becomes the initial champion with 1 life point.
* **Challengers appear:** Traverse the remaining elements as challengers.
* **Duel:**
  - If the challenger belongs to the same “sect” as the champion (same value), the champion gains 1 life point.
  - Otherwise, the champion loses 1 life point.
* **Champion replacement:**
   If the champion’s life drops to 0 (mutual defeat), the next challenger becomes the new champion with 1 life point.
* **Final survivor:**
   The person left on the stage at the end is the martial arts leader — the strict majority element.

**Why Is This Correct?**

Let the most frequent element (the strict majority) appear **a** times, and all other elements combined appear **b = n − a** times. The problem guarantees **a > b**.

**Key Observation**

Whenever the champion’s life drops to 0, it is equivalent to starting a new tournament — i.e., making a cut between `nums[i]` and `nums[i+1]`. This partitions the array into several segments.

Consider these segments one by one:

1. For every segment except the last

   Suppose within a segment:

   - The strict majority element appears **x** times
   - All other elements appear **y** times

   We must have **x ≤ y**.

   **Proof by contradiction:**
    If **x > y**, then the strict majority element has more “health” and cannot be eliminated by others. Its life would remain positive at the end of the segment, contradicting the assumption that the champion’s life reached 0.

   From this we get:

   ```java
   a − x > b − y
   ```

   Meaning that after removing this segment, the remaining counts still satisfy:

   ```
   a' > b'
   ```

   So in the remaining unprocessed elements, the strict majority element is still dominant.

   This argument applies repeatedly for each segment.

2. For the final segment

   Since **a' > b'** still holds, the strict majority element has more “health” than all others combined. Therefore, it cannot be eliminated, and its life must remain positive at the end.

   Hence, the final champion on the stage must be the strict majority eleme

TC: O(n)

SC: O(1)

```java
class Solution {
    public int majorityElement(int[] nums) {
        int result = 0;
        int hp = 0;
        for (int x : nums){
            if (hp == 0){ // x是初始擂主, 生命值为1
                result = x;
                hp = 1;
            }else{ // 比武, 同门加血,否则扣血   
                if (x == result){
                    hp = hp + 1;    
                }else{
                    hp = hp - 1;
                }
            }
        }   

        return result;
    }
}

// TC: O(n)
// SC: O(1)
```



