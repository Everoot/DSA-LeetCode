---
tags:
    - Hash Table
    - String
    - Breadth-First Search
    - Top Interviews
---

# [433. Minimum Genetic Mutation](https://leetcode.com/problems/minimum-genetic-mutation/)

A gene string can be represented by an 8-character long string, with choices from `'A'`, `'C'`, `'G'`, and `'T'`.

Suppose we need to investigate a mutation from a gene string `startGene` to a gene string `endGene`where one mutation is defined as one single character changed in the gene string.

- For example, `"AACCGGTT" --> "AACCGGTA"` is one mutation.

There is also a gene bank `bank` that records all the valid gene mutations. A gene must be in `bank` to make it a valid gene string.

Given the two gene strings `startGene` and `endGene` and the gene bank `bank`, return *the minimum number of mutations needed to mutate from* `startGene` *to* `endGene`. If there is no such a mutation, return `-1`.

Note that the starting point is assumed to be valid, so it might not be included in the bank.

 

**Example 1:**

```
Input: startGene = "AACCGGTT", endGene = "AACCGGTA", bank = ["AACCGGTA"]
Output: 1
```

**Example 2:**

```
Input: startGene = "AACCGGTT", endGene = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]
Output: 2
```

 

**Constraints:**

- `0 <= bank.length <= 10`
- `startGene.length == endGene.length == bank[i].length == 8`
- `startGene`, `endGene`, and `bank[i]` consist of only the characters `['A', 'C', 'G', 'T']`.



**Solution:**

```java
class Solution {
    static char[] DIRS = new char[]{'A', 'C', 'G', 'T'};
    public int minMutation(String startGene, String endGene, String[] bank) {
        Set<String> bankSet = new HashSet<>();

        for (String b : bank){
            bankSet.add(b);
        }

        if (!bankSet.contains(endGene)){
            return -1;
        }

        int result = 0;
        Deque<String> queue = new ArrayDeque<>();
        queue.offerLast(startGene);

        Set<String> visited = new HashSet<>();
        visited.add(startGene);

        while(!queue.isEmpty()){
            int size = queue.size();
            for (int i = 0; i < size; i++){
                String cur = queue.pollFirst();
                if (cur.equals(endGene)){
                    return result;
                }

                char[] curChar = cur.toCharArray();

                for (int j = 0; j < curChar.length; j++){
                    char old = curChar[j];
                    for (char c : DIRS){
                        if (c == old){
                            continue;
                        }

                        curChar[j] = c;

                        String next = new String(curChar);

                        if (bankSet.contains(next) && !visited.contains(next)){
                            visited.add(next);
                            queue.offerLast(next);
                        }
                    }

                    curChar[j] = old;
                }

            }

            result++;
        }

        return -1;

    }
}

// TC: O(n)
// SC: O(n)
```

