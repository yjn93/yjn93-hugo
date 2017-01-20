+++
Categories = ["Algorithm"]
Description = ""
Tags = ["LeetCode", "Basic algorithm"]
date = "2017-01-13T17:12:53-05:00"
menu = "main"
title = "KMP-String Pattern Match"

+++

# KMP
KMP is a string searching algorithm searches for occurrences of a "word" W within a main "text string" S by employing the observation that when a mismatch occurs, the word itself embodies sufficient information to determine where the next match could begin, thus bypassing re-examination of previously matched characters.

- [A link with detailed explanation] (http://blog.csdn.net/v_july_v/article/details/7041827)


## Calculate Longest Prefix Suffix (LPS) Array

```
    public int[] kmp(String str) {
        int len = str.length();
        int[] LPS = new int[len];
        LPS[0] = 0;
        int k = 0, j = 1;
        while(j < len) {
            if(str.charAt(k) == str.charAt(j))
                LPS[j ++] = ++ k;
            else if(k == 0)
                LPS[j ++] = 0;
            else
                k = LPS[k-1];
        }
		return LPS;
    }

```

## Calculate Next Array

```
    public int[] kmp(String str) {
        int len = str.length();
        int[] next = new int[len];
        next[0] = -1;
        int k = -1, j = 0;
        while(j < len-1) {
            if(k == -1 || str.charAt(k) == str.charAt(j))
                next[++ j] = ++ k;
            else 
                k = next[k];
        }
        return next;
    }

```

## Optimised Next Array

```
    public int[] kmp(String str) {
        int len = str.length();
        int[] next = new int[len];
        next[0] = -1;
        int k = -1, j = 0;
        while(j < len-1) {
            if(k == -1 || str.charAt(k) == str.charAt(j)) {
                if(str.charAt(++ k) == str.charAt(++ j))
                    next[j] = next[k];  //make sure str(j) != str(next[j])
                else    
                    next[j] = k;
            }
            else 
                k = next[k];
        }
        return next;
    }

```

# Encode String with Shortest Length

- [LeetCode 471](https://leetcode.com/problems/encode-string-with-shortest-length/) 

## Solution

dp[i][j] = string from index i to index j in encoded form.

We can write the following formula as:-
dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j]) or if we can find some pattern in string from i to j which will result in more less length.

Time Complexity = O(n^3)

```
    public String encode(String s) {
        if(s == null || s.length() < 5) return s;
        int len = s.length();
        String[][] dp = new String[len][len];
        
        for(int l = 0; l < len; l ++) {
            for(int i = 0; i < len-l; i ++) {
                int j = i + l;
                String str = s.substring(i, j + 1);
                dp[i][j] = str;
                if(l < 4) continue;
                for(int k = i; k < j; k ++) {
                    if(dp[i][k].length() + dp[k+1][j].length() < dp[i][j].length())
                        dp[i][j] = dp[i][k] + dp[k+1][j];
                }
                String pattern = kmp(str);
                if(pattern.length() == str.length()) continue;  // no repeat pattern found
                String patternEncode = str.length()/pattern.length() + "[" + dp[i][i+pattern.length()-1] + "]";
                if(patternEncode.length() < dp[i][j].length())
                    dp[i][j] = patternEncode;
            }
        }
        System.out.println(dp[0][7]);
        return dp[0][len-1];
    }
    
    public String kmp(String str) {
        int len = str.length();
        int[] LPS = new int[len];
        LPS[0] = 0;
        int k = 0, j = 1;
        while(j < len) {
            if(str.charAt(k) == str.charAt(j))
                LPS[j ++] = ++ k;
            else if(k == 0)
                LPS[j ++] = 0;
            else
                k = LPS[k-1];
        }
        String pattern = str.substring(LPS[len-1]);
        if(pattern.length() != len && len % pattern.length() == 0)
            return pattern;
        return str;
    }

```

