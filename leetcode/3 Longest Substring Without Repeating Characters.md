# Longest Substring Without Repeating Characters
[Markdowm 数学公式输入](http://www.cnblogs.com/q735613050/p/7253073.html)
## Description
Given a string, find the length of the **longest substring** without repeating characters.

### Examples:

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

## Solution

### Approach #1 Brute Force[Time limit Exceeded]

#### Intuition

Check all the substring one by one to see if it has no duplicate character.

#### Algorithm
Suppose we have a function boolean allUnique(String substring) which will return true if the characters in the substring are all unique, otherwise false. We can iterate through all the possible substrings of the given string s and call the function allUnique. If it turns out to be true, then we update our answer of the maximum length of substring without duplicate characters.

Now let's fill the missing parts:

1. To enumerate all substrings of a given string, we enumerate the start and end indices of them. Suppose the start and end indices are ii and jj, respectively. Then we have 0 ≤ i < j ≤ n (here end index j is exclusive by convention). Thus, using two nested loops with i from 0 to n−1 and j from i+1 to n, we can enumerate all the substrings of s.

2. To check if one string has duplicate characters, we can use a set. We iterate through all the characters in the string and put them into the set one by one. Before putting one character, we check if the set already contains it. If so, we return false. After the loop, we return true.


#### JAVA
```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j <= n; j++)
                if (allUnique(s, i, j)) ans = Math.max(ans, j - i);
        return ans;
    }

    public boolean allUnique(String s, int start, int end) {
        Set<Character> set = new HashSet<>();
        for (int i = start; i < end; i++) {
            Character ch = s.charAt(i);
            if (set.contains(ch)) return false;
            set.add(ch);
        }
        return true;
    }
}

```
#### Complexity Analysis
- Time complexity: $O(n^3)$

To verify if characters within index range $[i,j)$ are all unique, we need to scan all of them. Thus, it costs $O(j - i)$ time.

For a given i, the sum of time costed by each $j \in [i+1, n]$ is
$$\sum_{i+1}^{n}O(j - i))$$

Thus, the sum of all the time consumption is: 
$$ O\left(\sum_{i = 0}^{n - 1}\left(\sum_{j = i + 1}^{n}(j - i)\right)\right) = O\left(\sum_{i = 0}^{n - 1}\frac{(1 + n - i)(n - i)}{2}\right) = O(n^3)$$

### Aproach #2 Sliding Windows [Accepted]
####

```python
 class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s) < 2:
            return len(s)
        longest = 0;
        substring = list()
        for i in range(len(s)):
            substring.append(s[i])
            if longest < len(substring):
                longest = len(substring)
            if i +1 < len(s) and s[i + 1] in substring:
                substring = substring[substring.index(s[i+1])+1:]
        return longest
```

