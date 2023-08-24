### Leetcode 3.无重复字符的最长子串

[3. 无重复字符的最长子串 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

给定一个字符串 **s**，请你找出其中不含有重复字符的**最长字串**的长度。

#### 1.双指针 + 哈希表

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] ss = new int[128];
        int mod = 128;
        char[] chars = s.toCharArray();
        int n = chars.length;
        int res = 0;
        if (s == null) return 0;
        for (int i = 0, j = 0 ; i < n ; i ++) {
            //可能会出现负数
            int u = (chars[i] - 'a' + mod) % mod;
            ss[u] ++;
            //如果出现重复
            while (ss[u] > 1) {
                ss[(chars[j] - 'a' + mod) % mod] --;
                j ++;
            }
            res = Math.max(res, i - j + 1);
        }
        return res;
    }
}
```

#### 2.滑动窗口

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();
        char[] strs = s.toCharArray();
        int n = strs.length;
        int res = 0;
        for (int i = 0, j = 0 ; i < n ; i ++) {
            if (i != 0) {
                set.remove(strs[i - 1]);
            }
            while (j < n && !set.contains(strs[j])) {
                set.add(strs[j]);
                j ++;
            }
            res = Math.max(res, j - i);
        }
        return res;

    }
}
```

