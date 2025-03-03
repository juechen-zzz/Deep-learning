[TOC]

## 最长回文子串   

> 5
>
> *给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。*
>
> *输入: "babad"*
>
> *输出: "bab"*

```java
// DP
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        char[] cur = s.toCharArray();
        String ans = "";

        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (i == j) {
                    dp[i][j] = true;
                }
                else if (j - i == 1) {
                    dp[i][j] = (cur[i] == cur[j]);
                }
                else {
                    dp[i][j] = (dp[i + 1][j - 1] && (cur[i] == cur[j]));
                }

                if (dp[i][j] && j - i + 1 > ans.length()) {
                    ans = s.substring(i, j + 1);
                }
            }
        }

        return ans;
    }
}
```

> 647
>
> 给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。
>
> 具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

```java
class Solution {
    public int countSubstrings(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        int ans = 0;

        for (int j = 0; j < n; j++) {
            for (int i = 0; i <= j; i++) {
                if (s.charAt(i) != s.charAt(j)) {continue;}
                dp[i][j] = (j - i <= 2 || dp[i + 1][j - 1]);
                if (dp[i][j]) {ans++;}
            }
        }

        return ans;
    }
}
```

## 最长回文子序列

> 516
>
> 给定一个字符串 `s` ，找到其中最长的回文子序列，并返回该序列的长度。可以假设 `s` 的最大长度为 `1000` 。

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];
        for (int i = 0; i < n; i++) {dp[i][i] = 1;}

        for (int i = n - 2; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                }
                else {
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[0][n - 1];
    }
}
```

## 最长递增子序列

> 300
>
> *给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。*
>
> *子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。*

```java
// DP
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }

        return Arrays.stream(dp).max().getAsInt();
    }
}

// 二分
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] top = new int[nums.length];
        
        int piles = 0;
        for (int i = 0; i < nums.length; i++){
            int poker = nums[i];

            int left = 0, right = piles;
            while (left < right){
                int mid = (left + right) / 2;
                if (top[mid] > poker){
                    right = mid;
                } else if (top[mid] < poker){
                    left = mid + 1;
                }else {
                    right = mid;
                }
            }

            if (left == piles) {piles++;}
            top[left] = poker;
        }
        return piles;
    }
}
```

## 最长递增子序列的个数

> 673
>
> *给定一个未排序的整数数组，找到最长递增子序列的个数。*

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        int n = nums.length;
        if (n <= 1) {return n;}
        int[] length = new int[n];			// 记录当前数为结尾的最长子序列长度
        int[] count = new int[n];			// 记录当前数为结尾的最长子序列个数
        Arrays.fill(count, 1);

        for (int j = 0; j < n; j++) {
            for (int i = 0; i < j; i++) {
                if (nums[i] < nums[j]) {
                    if (length[i] >= length[j]) {
                        length[j] = length[i] + 1;
                        count[j] = count[i];
                    }
                    else if (length[i] + 1 == length[j]) {
                        count[j] += count[i];
                    }
                }
            }
        }

        int ans = 0;
        int longest = Arrays.stream(length).max().getAsInt();
        for (int i = 0; i < n; i++) {
            if (length[i] == longest) {
                ans += count[i];
            }
        }

        return ans;
    }
}
```

## 最长公共子序列

> 1143
>
> *给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。*
>
> *一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。*
>
> *例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。*
>
> *若这两个字符串没有公共子序列，则返回 0。*

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        
        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                if (i == 0 || j == 0) {
                    continue;
                }
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }
                else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[m][n];
    }
}
```

## 最长重复子数组

> 718
>
> *给两个整数数组 A 和 B ，返回两个数组中公共的、长度最长的子数组的长度。*
>
> A: [1,2,3,2,1]
> B: [3,2,1,4,7]
> 输出：3

```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int n = nums1.length, m = nums2.length;
        int[][] dp = new int[n + 1][m + 1];
        int ans = 0;

        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                dp[i][j] = (nums1[i] == nums2[j]) ? dp[i + 1][j + 1] + 1 : 0;
                ans = Math.max(ans, dp[i][j]);
            }
        }

        return ans;
    }
}
```

## 使序列递增的最小交换次数

> 801
>
> *我们有两个长度相等且不为空的整型数组 A 和 B 。*
>
> *我们可以交换 A[i] 和 B[i] 的元素。注意这两个元素在各自的序列中应该处于相同的位置。*
>
> *在交换过一些元素之后，数组 A 和 B 都应该是严格递增的（数组严格递增的条件仅为A[0] < A[1] < A[2] < ... < A[A.length - 1]）。*
>
> *给定数组 A 和 B ，请返回使得两个数组均保持严格递增状态的最小交换次数。假设给定的输入总是有效的。*

```java
// 在判断 A[i] 和 B[i] 是否交换时，只需要考虑它们之前的一个元素 A[i - 1] 和 B[i - 1] 是否被交换就可以了。
// n1 表示数组 A 和 B 满足前 i - 1 个元素分别严格递增，并且 A[i - 1] 和 B[i - 1] 未被交换的最小交换次数
// s1 表示 A[i - 1] 和 B[i - 1] 被交换的最小交换次数
class Solution {
    public int minSwap(int[] A, int[] B) {
        // n: natural, s: swapped
        int n1 = 0, s1 = 1;
        for (int i = 1; i < A.length; ++i) {
            int n2 = Integer.MAX_VALUE, s2 = Integer.MAX_VALUE;
            // 如果 a1 和 b1 未被交换，a2 和 b2 就可以不交换；
            // 同样如果 a1 和 b1 被交换了，a2 和 b2 在交换后满足严格递增的条件
            if (A[i - 1] < A[i] && B[i - 1] < B[i]) {
                n2 = n1;
                s2 = s1 + 1;
            }
            if (A[i - 1] < B[i] && B[i - 1] < A[i]) {
                n2 = Math.min(n2, s1);
                s2 = Math.min(s2, n1 + 1);
            }
            n1 = n2;
            s1 = s2;
        }
        return Math.min(n1, s1);
    }
}
```

## 最长有效括号

> 32
>
> *给你一个只包含 '(' 和 ')' 的字符串，找出最长有效（格式正确且连续）括号子串的长度。*
>
> *输入：s = "(()"*
>
> *输出：2*

```java
class Solution {
    public int longestValidParentheses(String s) {
        int n = s.length();
        if (n == 0) {
            return 0;
        }
        
        int[] dp = new int[n];
        for (int i = 1; i < n; i++) {
            if (s.charAt(i) == ')' && i - dp[i - 1] - 1 >= 0 && s.charAt(i - dp[i - 1] - 1) == '(') {
                dp[i] = dp[i - 1] + 2;
                if (i - dp[i - 1] - 2 >= 0) {
                    dp[i] += dp[i - dp[i - 1] - 2];
                }
            }
        }

        return Arrays.stream(dp).max().getAsInt();
    }
}
```

## 摆动序列

> 376
>
> [1,7,4,9,2,5] 是一个摆动序列，因为差值 (6,-3,5,-7,3) 是正负交替出现的。相反, [1,4,7,2,5] 和 [1,7,4,5,5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。
>
> 给定一个整数序列，返回作为摆动序列的最长子序列的长度。 通过从原始序列中删除一些（也可以不删除）元素来获得子序列，剩下的元素保持其原始顺序。

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int n = nums.length;
        if (n < 2) {return n;}

        int[] up = new int[n];
        int[] down = new int[n];
        up[0] = down[0] = 1;
        
        for (int i = 1; i < n; i++) {
            if (nums[i] > nums[i - 1]) {
                up[i] = Math.max(up[i - 1], down[i - 1] + 1);
                down[i] = down[i - 1];
            }
            else if (nums[i] < nums[i - 1]) {
                up[i] = up[i - 1];
                down[i] = Math.max(down[i - 1], up[i - 1] + 1);
            }
            else {
                up[i] = up[i - 1];
                down[i] = down[i - 1];
            }
        }

        return Math.max(up[n - 1], down[n - 1]);
    }
}
```

## 最长湍流子数组

> 978
>
> *当 A 的子数组 A[i], A[i+1], ..., A[j] 满足下列条件时，我们称其为湍流子数组：*
>
> *若 i <= k < j，当 k 为奇数时， A[k] > A[k+1]，且当 k 为偶数时，A[k] < A[k+1]；*
>
> *或 若 i <= k < j，当 k 为偶数时，A[k] > A[k+1] ，且当 k 为奇数时， A[k] < A[k+1]。*
>
> *也就是说，如果比较符号在子数组中的每个相邻元素对之间翻转，则该子数组是湍流子数组。*
>
> *返回 A 的最大湍流子数组的长度。*

```java
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        if (arr.length == 1) {return 1;}
        int n = arr.length;
        int[][] dp = new int[n][2];
        dp[0][0] = dp[0][1] = 1;

        for (int i = 1; i < n; i++) {
            dp[i][0] = dp[i][1] = 1;
            if (arr[i - 1] > arr[i]) {dp[i][0] = dp[i - 1][1] + 1;}
            else if (arr[i - 1] < arr[i]) {dp[i][1] = dp[i - 1][0] + 1;}
        }

        int ans = 1;
        for (int i = 0; i < n; i++) {
            ans = Math.max(ans, Math.max(dp[i][0], dp[i][1]));
        }
        return ans;
    }
}
```

## 摆动序列

> *如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为摆动序列。第一个差（如果存在的话）可能是正数或负数。少于两个元素的序列也是摆动序列。*
>
> *例如， [1,7,4,9,2,5] 是一个摆动序列，因为差值 (6,-3,5,-7,3) 是正负交替出现的。相反, [1,4,7,2,5] 和 [1,7,4,5,5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。*
>
> *给定一个整数序列，返回作为摆动序列的最长子序列的长度。 通过从原始序列中删除一些（也可以不删除）元素来获得子序列，剩下的元素保持其原始顺序。*

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int n = nums.length;
        if (n < 2) {return n;}

        int[][] dp = new int[n][2];
        dp[0][0] = 1;
        dp[0][1] = 1;

        for (int i = 1; i < n; i++) {
            if (nums[i] > nums[i - 1]) {
                dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + 1);
                dp[i][1] = dp[i - 1][1];
            }
            else if (nums[i] < nums[i - 1]) {
                dp[i][0] = dp[i - 1][0];
                dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + 1);
            }
            else {
                dp[i][0] = dp[i - 1][0];
                dp[i][1] = dp[i - 1][1];
            }
        }

        return Math.max(dp[n - 1][0], dp[n - 1][1]);
    }
}
```

## 最长数对链

> 646
>
> *给出 n 个数对。 在每一个数对中，第一个数字总是比第二个数字小。*
>
> *现在，我们定义一种跟随关系，当且仅当 b < c 时，数对(c, d) 才可以跟在 (a, b) 后面。我们用这种形式来构造一个数对链。*
>
> *给定一个数对集合，找出能够形成的最长数对链的长度。你不需要用到所有的数对，你可以以任何顺序选择其中的一些数对来构造。*

```java
class Solution {
    public int findLongestChain(int[][] pairs) {
        Arrays.sort(pairs, (a, b) -> a[0] - b[0]);
        int n = pairs.length;
        
        int[] dp = new int[n];
        Arrays.fill(dp, 1);

        for (int j = 1; j < n; j++) {
            for (int i = 0; i < j; i++) {
                if (pairs[i][1] < pairs[j][0]) {
                    dp[j] = Math.max(dp[j], dp[i] + 1);
                }
            }
        }

        return Arrays.stream(dp).max().getAsInt();
    }
}
```

## 两个字符串的删除操作

> 583
>
> *给定两个单词 word1 和 word2，找到使得 word1 和 word2 相同所需的最小步数，每步可以删除任意一个字符串中的一个字符。*

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int n = word1.length(), m = word2.length();
        int[][] dp = new int[n + 1][m + 1];

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                if (i == 0 || j == 0) {dp[i][j] = i + j;}
                else if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                else {
                    dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + 1;
                }
            }
        }

        return dp[n][m];
    }
}
```

## 两个字符串的最小ASCII删除和

> 712
>
> *给定两个字符串s1, s2，找到使两个字符串相等所需删除字符的ASCII值的最小和。*

```java
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int n = s1.length(), m = s2.length();
        int[][] dp = new int[n + 1][m + 1];

        for (int i = n - 1; i >= 0; i--) {
            dp[i][m] = dp[i + 1][m] + s1.codePointAt(i);
        }
        for (int j = m - 1; j >= 0; j--) {
            dp[n][j] = dp[n][j + 1] + s2.codePointAt(j);
        }

        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                if (s1.charAt(i) == s2.charAt(j)) {
                    dp[i][j] = dp[i + 1][j + 1];
                }
                else {
                    dp[i][j] = Math.min(dp[i + 1][j] + s1.codePointAt(i), dp[i][j + 1] + s2.codePointAt(j));
                }
            }
        }

        return dp[0][0];
    }
}
```

## 正则表达式匹配

> 10
>
> *给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '\*' 的正则表达式匹配。*
>
> *'.' 匹配任意单个字符*
>
> *'\*' 匹配零个或多个前面的那一个元素*
>
> *输入：s = "ab" p = ".\*"*
>
> *输出：true*

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] dp = new boolean[m + 1][n + 1];
        dp[0][0] = true;

        // 当为*时，可以匹配*之前的字符0次，也就是每次多余两个字符
        for (int k = 2; k <= n; k++) {
            dp[0][k] = dp[0][k - 2] && (p.charAt(k - 1) == '*');
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (p.charAt(j) == '*') {
                  	// 1 匹配0次时，直接等于p删两个字符 dp[i + 1][j - 1]
                    // 2 匹配多次时，等于在s的最后加上一个p中*前面的字符，所以比较s[i]和p[j-1]，同时dp[i][j + 1]
                    dp[i + 1][j + 1] = (dp[i + 1][j - 1] || (dp[i][j + 1] && headMatched(s, p, i, j - 1)));
                }
                else {
                    dp[i + 1][j + 1] = (dp[i][j] && headMatched(s, p, i, j));
                }
            }
        }

        return dp[m][n];
    }

    //判断s第i个字符和p第j个字符是否匹配
    private boolean headMatched(String s, String p, int i, int j){
        return (s.charAt(i) == p.charAt(j) || p.charAt(j) == '.');
    }
}
```

## 通配符匹配

> 44
>
> *给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 '\*' 的通配符匹配。*
>
> *'?' 可以匹配任何单个字符。*
>
> *'\*' 可以匹配任意字符串（包括空字符串）。*
>
> *两个字符串完全匹配才算匹配成功。*

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int n = s.length(), m = p.length();
        boolean[][] dp = new boolean[n + 1][m + 1];
        dp[0][0] = true;

        for (int i = 1; i <= m; i++) {
            if (p.charAt(i - 1) == '*') {dp[0][i] = true;}
            else {break;}
        }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (p.charAt(j - 1) == '*') {
                    dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
                }
                else if (p.charAt(j - 1) == '?' || s.charAt(i - 1) == p.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
            }
        }

        return dp[n][m];
    }
}
```

## 最小路径和

> 64
>
> 给定一个包含非负整数的 `m x n` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
>
> **说明：**每次只能向下或者向右移动一步。

```java
class Solution {
    public int minPathSum(int[][] grid) {
        int n = grid.length, m = grid[0].length;
        int[][] dp = new int[n][m];
        dp[0][0] = grid[0][0];

        for (int i = 0; i < n; i++) {
            if (i == 0) {continue;}
            else {dp[i][0] = dp[i - 1][0] + grid[i][0];}
        }

        for (int j = 0; j < m; j++) {
            if (j == 0) {continue;}
            else {dp[0][j] = dp[0][j - 1] + grid[0][j];}
        }

        if (n == 1 || m == 1) {return dp[n - 1][m - 1];}
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {
                dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }

        return dp[n - 1][m - 1];
    }
}
```

## 三角形最小路径和

> 120
>
> *给定一个三角形 triangle ，找出自顶向下的最小路径和。*

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[][] dp = new int[n][n];
        dp[0][0] = triangle.get(0).get(0);
        for (int i = 1; i < n; i++) {
            for (int j = 0; j <= i; j++) {
                if (j == 0) {
                    dp[i][j] = triangle.get(i).get(j) + dp[i - 1][j];
                }
                else if (j == i) {
                    dp[i][j] = triangle.get(i).get(j) + dp[i - 1][j - 1];
                }
                else {
                    dp[i][j] = triangle.get(i).get(j) + Math.min(dp[i - 1][j - 1], dp[i - 1][j]);
                }
            }
        }

        return Arrays.stream(dp[n - 1]).min().getAsInt();
    }
}
```

## 编辑距离

> 72
>
> *给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。*
>
> 对一个单词进行如下三种操作：*
>
> *插入一个字符*
>
> *删除一个字符*
>
> *替换一个字符*

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int n = word1.length(), m = word2.length();
        int[][] dp = new int[n + 1][m + 1];
        for (int i = 1; i <= n; i++) {dp[i][0] = i;}
        for (int j = 1; j <= m; j++) {dp[0][j] = j;}

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                else {
                    dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
                }
            }
        }

        return dp[n][m];
    }
}
```

## 不同的子序列

> 115
>
> 给定一个字符串 `s` 和一个字符串 `t` ，计算在 `s` 的子序列中 `t` 出现的个数。

```java
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length(), n = t.length();
        if (m < n) {return 0;}

        // dp[i][j]表示在s[i:]的子序列中t[j:]出现的个数
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 0; i <= m; i++) {dp[i][n] = 1;}

        for (int i = m - 1; i >= 0; i--) {
            char sChar = s.charAt(i);
            for (int j = n - 1; j >= 0; j--) {
                char tChar = t.charAt(j);
                if (sChar == tChar) {
                    dp[i][j] = dp[i + 1][j + 1] + dp[i + 1][j];
                } else {
                    dp[i][j] = dp[i + 1][j];
                }
            }
        }
        
        return dp[0][0];
    }
}
```

## 计算各个位数不同的数字个数

> 357
>
> *给定一个非负整数 n，计算各位数字都不同的数字 x 的个数，其中 0 ≤ x < 10n 。*

```java
class Solution {
    /**
     * 排列组合：n位有效数字 = 每一位都从 0~9 中选择，且不能以 0 开头
     * 1位数字：0~9                      10
     * 2位数字：C10-2，且第一位不能是0      9 * 9
     * 3位数字：C10-3，且第一位不能是0      9 * 9 * 8
     * 4位数字：C10-4，且第一位不能是0      9 * 9 * 8 * 7
     * ... ...
     * 最后，总数 = 所有 小于 n 的位数个数相加
     */
    public int countNumbersWithUniqueDigits(int n) {
        if (n == 0) {return 1;}
        int first = 10, second = 9 * 9;
        int size = Math.min(n, 10);
        for (int i = 2; i <= size; i++) {
            first += second;
            second *= (10 - i);
        }
        return first;
    }
}
```

## 乘积最大子数组

> 152
>
> *给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。*

```java
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int[] maxDp = new int[n];
        int[] minDp = new int[n];
        maxDp[0] = nums[0];
        minDp[0] = nums[0];

        for (int i = 1; i < n; i++) {
            maxDp[i] = Math.max(nums[i], Math.max(maxDp[i - 1] * nums[i], minDp[i - 1] * nums[i]));
            minDp[i] = Math.min(nums[i], Math.min(maxDp[i - 1] * nums[i], minDp[i - 1] * nums[i]));
        }

        return Arrays.stream(maxDp).max().getAsInt();
    }
}
```

## 地下城游戏

> 174
>
> *编写一个函数来计算确保骑士能够拯救到公主所需的最低初始健康点数。*

```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        int n = dungeon.length, m = dungeon[0].length;
        int[][] dp = new int[n + 1][m + 1];
        for (int i = 0; i <= n; i++) {Arrays.fill(dp[i], Integer.MAX_VALUE);}
        dp[n][m - 1] = dp[n - 1][m] = 1;

        // needMin + globalDun[i][j] = Math.min(dp[i + 1][j], dp[i][j + 1])
        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                int minCur = Math.min(dp[i + 1][j], dp[i][j + 1]);
                dp[i][j] = Math.max(minCur - dungeon[i][j], 1);
            }
        }

        return dp[0][0];
    }
}
```

## 最大正方形

> 221
>
> *在一个由 '0' 和 '1' 组成的二维矩阵内，找到只包含 '1' 的最大正方形，并返回其面积。*

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {return 0;}
        int n = matrix.length, m = matrix[0].length;
        int[][] dp = new int[n][m];

        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == '1') {
                    if (i == 0 || j == 0) {dp[i][j] = 1;}
                    else {
                        dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
                    }
                    ans = Math.max(ans, dp[i][j]);
                }
            }
        }

        return ans * ans;
    }
}
```

## 二维区域检索求和

> 304
>
> 给定一个二维矩阵，计算其子矩形范围内元素的总和，该子矩阵的左上角为 `(row1, col1)` ，右下角为 `(row2, col2)` 。

```java
class NumMatrix {
    int[][] dp;

    public NumMatrix(int[][] matrix) {
        int m = matrix.length;
        if (m > 0) {
            int n = matrix[0].length;
            dp = new int[m + 1][n + 1];
            dp[1][1] = matrix[0][0];
            for (int i = 1; i < m; i++) {dp[i + 1][1] = dp[i][1] + matrix[i][0];}
            for (int j = 1; j < n; j++) {dp[1][j + 1] = dp[1][j] + matrix[0][j];}
            for (int i = 1; i < m; i++) {
                for (int j = 1; j < n; j++) {
                    dp[i + 1][j + 1] = matrix[i][j] + dp[i][j + 1] + dp[i + 1][j] - dp[i][j];
                }
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return dp[row2 + 1][col2 + 1] + dp[row1][col1] - dp[row2 + 1][col1] - dp[row1][col2 + 1];
    }
}
```

## 零钱兑换

> 322
>
> *给定不同面额的硬币 coins 和一个总金额 amount。*
>
> *编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。*
>
> *可以认为每种硬币的数量是无限的。*

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;

        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }

        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

> 518
>
> *给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。* 

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] += dp[i - coin];
            }
        }

        return dp[amount];
    }
}
```

## 解码方法

> 91
>
> *'A' -> 1*
>
> *'B' -> 2*
>
> *...*
>
> *'Z' -> 26*
>
> *要 解码 已编码的消息，所有数字必须基于上述映射的方法，反向映射回字母（可能有多种方法）。例如，"111" 可以将 "1" 中的每个 "1" 映射为 "A" ，从而得到 "AAA" ，或者可以将 "11" 和 "1"（分别为 "K" 和 "A" ）映射为 "KA" 。注意，"06" 不能映射为 "F" ，因为 "6" 和 "06" 不同。*
>
> *给你一个只含数字的 非空 字符串 num ，请计算并返回 解码 方法的 总数 。*

```java
class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        int[] dp = new int[n + 1];
        dp[0] = 1;

        for (int i = 1; i <= n; i++) {
            if (s.charAt(i - 1) != '0') {
                dp[i] += dp[i - 1];
            }
            if (i > 1 && s.charAt(i - 2) != '0' && (s.charAt(i - 2) - '0') * 10 + (s.charAt(i - 1) - '0') <= 26) {
                dp[i] += dp[i - 2];
            }
        }

        return dp[n];
    }
}
```

## 俄罗斯套娃

> 354
>
> *给定一些标记了宽度和高度的信封，宽度和高度以整数对形式 (w, h) 出现。当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。*
>
> *请计算最多能有多少个信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。*

```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        int n = envelopes.length;
        Arrays.sort(envelopes, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);

        int[] height = new int[n];
        for (int i = 0; i < n; i++) {height[i] = envelopes[i][1];}
        
        return myMethod(height);
    }

    // 对于每一种 w 值，其对应的信封在排序后的数组中是按照 h 值递减的顺序出现的，那么这些 h 值不可能组成长度超过 1 的严格递增的序列
    private static int myMethod(int[] height) {
        int n = height.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (height[i] > height[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }

        return Arrays.stream(dp).max().getAsInt();
    }
}
```

## 最大整除子集

> 368
>
> *给出一个由无重复的正整数组成的集合，找出其中最大的整除子集，子集中任意一对 (Si，Sj) 都要满足：Si % Sj = 0 或 Sj % Si = 0。*
>
> *如果有多个目标子集，返回其中任何一个均可。*

```java
 class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        int n = nums.length;
        if (n == 0) {return new ArrayList<>();}
        Arrays.sort(nums);

        // dp[i]表示必选nums[i]的情况下的最大整除子集
        List[] dp = new List[n];
        dp[0] = new ArrayList<>();
        dp[0].add(nums[0]);

        List<Integer> ans = dp[0];
        for (int i = 1; i < n; i++) {
            dp[i] = new ArrayList<>();
            dp[i].add(nums[i]);

            // 若nums[i]整除nums[j]，说明可以将dp[j]+nums[i]作为dp[i]
            for (int j = 0; j < i; j++) {
                if (nums[i] % nums[j] == 0 && dp[i].size() < dp[j].size() + 1) {
                    dp[i] = new ArrayList<>(dp[j]);
                    dp[i].add(nums[i]);
                }
            }

            if (ans.size() < dp[i].size()) {ans = dp[i];}
        }

        return ans;
    }
}
```

## 猜数字大小

> 375
>
> *我们正在玩一个猜数游戏，游戏规则如下：*
>
> *我从 1 到 n 之间选择一个数字，你来猜我选了哪个数字。*
>
> *每次你猜错了，我都会告诉你，我选的数字比你的大了或者小了。*
>
> *然而，当你猜了数字 x 并且猜错了的时候，你需要支付金额为 x 的现金。直到你猜到我选的数字，你才算赢得了这个游戏。*

```java
class Solution {
    public int getMoneyAmount(int n) {
        if (n == 1) {return 0;}
        int[][] dp = new int[n + 1][n + 1];

        // dp[i][j]表示从(j,i)区间选择内的最小值，选定区间后，在用k划分成子问题
        for (int i = 1; i <= n; i++) {
            for (int j = i - 1; j > 0; j--) {
                int curMin = Integer.MAX_VALUE, left = 0, right = 0;
                for (int k = i; k > j - 1; k--) {
                    left = (k == j || k - 1 == j) ? -1 : dp[k - 1][j];
                    right = (k == i || k + 1 == i) ? -1 : dp[i][k + 1];
                    curMin = (left == -1 && right == -1) ? k : Math.min(curMin, k + Math.max(left, right));
                }
                dp[i][j] = curMin;
            }
        }
        return dp[n][1];
    }
}
```

## 分割数组的最大值

> 410
>
> *给定一个非负整数数组 nums 和一个整数 m ，你需要将这个数组分成 m 个非空的连续子数组。*
>
> *设计一个算法使得这 m 个子数组各自和的最大值最小。*

```java
class Solution {
    public int splitArray(int[] nums, int m) {
        int n = nums.length;
        // dp[i][j]表示前i个数字分成j组的最大值
        int[][] dp = new int[n + 1][m + 1];
        for (int i = 0; i <= n; i++) {Arrays.fill(dp[i], Integer.MAX_VALUE);}

        // 记录和，这样能快速算出sum(i, j)
        int[] sum = new int[n + 1];
        for (int i = 0; i < n; i++) {sum[i + 1] = sum[i] + nums[i];}

        dp[0][0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= Math.min(i, m); j++) {
                // 遍历k位置
                for (int k = 0; k < i; k++) {
                    dp[i][j] = Math.min(dp[i][j], Math.max(dp[k][j - 1], sum[i] - sum[k]));
                }
            }
        }

        return dp[n][m];
    }
}
```

## 分割等和子集

> 416
>
> *给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。*

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int n = nums.length;
        if (n < 2) {return false;}

        int sum = Arrays.stream(nums).sum();
        int maxNum = Arrays.stream(nums).max().getAsInt();
        
        if (sum % 2 == 1) {return false;}
        if (maxNum > sum / 2) {return false;}
        int target = sum / 2;

        // dp[i][j]表示从数组的(0,i)范围内取若干个数，是否存在一种方案的和为j
        boolean[][] dp = new boolean[n][target + 1];
        for (int i = 0; i < n; i++) {dp[i][0] = true;}
        dp[0][nums[0]] = true;

        for (int i = 1; i < n; i++) {
            int curNum = nums[i];
            for (int j = 1; j <= target; j++) {
                if (j >= curNum) {
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - curNum];
                }
                else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }

        return dp[n - 1][target];
    }
}
```

## 一和零

> 474   
>
> 给你一个二进制字符串数组 strs 和两个整数 m 和 n 。
>
> 请你找出并返回 strs 的最大子集的大小，该子集中 最多 有 m 个 0 和 n 个 1 。
>
> 如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。

```java
class Solution {
    // 可以理解为二维的背包问题,dp[i][j]表示有i个0,j个1的情况下的最大子集
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        for (String cur : strs) {
            int[] count = myMethod(cur);
            for (int zero = m; zero >= count[0]; zero--) {
                for (int one = n; one >= count[1]; one--) {
                    dp[zero][one] = Math.max(dp[zero][one], 1 + dp[zero - count[0]][one - count[1]]);
                }
            }
        }
        return dp[m][n];
    }

    private static int[] myMethod(String s) {
        int[] count = new int[2];
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '0') {count[0]++;}
            else {count[1]++;}
        }
        return count;
    }
}
```

## 目标和

> 494
>
> 给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。
>
> 返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int[][] dp = new int[nums.length][2001];
        dp[0][nums[0] + 1000] = 1;
        dp[0][-nums[0] + 1000] += 1;

        for (int i = 1; i < nums.length; i++) {
            for (int sum = -1000; sum <= 1000; sum++) {
                if (dp[i - 1][sum + 1000] > 0) {
                    dp[i][sum + nums[i] + 1000] += dp[i - 1][sum + 1000];
                    dp[i][sum - nums[i] + 1000] += dp[i - 1][sum + 1000];
                }
            }
        }

        return S > 1000 ? 0 : dp[nums.length - 1][S + 1000];
    }
}
```

## 环绕字符串中唯一的子字符串

> 467
>
> 把字符串 s 看作是“abcdefghijklmnopqrstuvwxyz”的无限环绕字符串，所以 s 看起来是这样的："...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd....". 
>
> 现在我们有了另一个字符串 p 。你需要的是找出 s 中有多少个唯一的 p 的非空子串，尤其是当你的输入是字符串 p ，你需要输出字符串 s 中 p 的不同的非空子串的数目。 

```java
class Solution {
    public int findSubstringInWraproundString(String p) {
        int[] dp = new int[26];
        char[] array = p.toCharArray();

        int count = 0;
        for (int i = 0; i < array.length; i++) {
            if (i > 0 && (array[i] - array[i - 1] - 1) % 26 == 0) {count++;}
            else {count = 1;}

            dp[array[i] - 'a'] = Math.max(dp[array[i] - 'a'], count);
        }

        return Arrays.stream(dp).sum();
    }
}
```

## 预测赢家/石子游戏

> 486 877
>
> *给定一个表示分数的非负整数数组。 玩家 1 从数组任意一端拿取一个分数，随后玩家 2 继续从剩余数组任意一端拿取分数，然后玩家 1 拿，…… 。每次一个玩家只能拿取一个分数，分数被拿取之后不再可取。直到没有剩余分数可取时游戏结束。最终获得分数总和最多的玩家获胜。*
>
> *给定一个表示分数的数组，预测玩家1是否会成为赢家。你可以假设每个玩家的玩法都会使他的分数最大化。*

```java
// 回溯
class Solution {
    public boolean PredictTheWinner(int[] nums) {
        return myMethod(nums, 0, nums.length - 1, 1) >= 0;
    }

    private int myMethod(int[] nums, int start, int end, int turn) {
        if (start == end) {
            return nums[start] * turn;
        }
        int scoreStart = nums[start] * turn + myMethod(nums, start + 1, end, -turn);
        int scoreEnd = nums[end] * turn + myMethod(nums, start, end - 1, -turn);
        
        if(turn == 1) {return Math.max(scoreStart, scoreEnd);}
        else {return Math.min(scoreStart, scoreEnd);}
    }
}

// DP : dp[i][j]表示当数组剩下的部分为下标i到j时，当前玩家与另一个玩家的分数之差的最大值
class Solution {
    public boolean PredictTheWinner(int[] nums) {
        int n = nums.length;
        int[][] dp = new int[n][n];
        for (int i = 0; i < n; i++) {dp[i][i] = nums[i];}

        for (int i = n - 2; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                dp[i][j] = Math.max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1]);
            }
        }

        return dp[0][n - 1] >= 0;
    }
}
```

## 移除盒子

> 546
>
> *给出一些不同颜色的盒子，盒子的颜色由数字表示，即不同的数字表示不同的颜色。*
>
> *你将经过若干轮操作去去掉盒子，直到所有的盒子都去掉为止。每一轮你可以移除具有相同颜色的连续 k 个盒子（k >= 1），这样一轮之后你将得到 k \* k 个积分。*
>
> *当你将所有盒子都去掉之后，求你能获得的最大积分和。*

```java
class Solution {
    int[][][] dp;

    public int removeBoxes(int[] boxes) {
        int n = boxes.length;
        dp = new int[n][n][n];
        return calculate(boxes, 0, n - 1, 0);
    }

    private int calculate(int[] boxes, int left, int right, int k) {
        if (left > right) {return 0;}

        if (dp[left][right][k] == 0) {
            dp[left][right][k] = calculate(boxes, left, right - 1, 0) + (k + 1) * (k + 1);
            for (int i = left; i < right; i++) {
                if (boxes[i] == boxes[right]) {
                    dp[left][right][k] = Math.max(dp[left][right][k], calculate(boxes, left, i, k + 1) + calculate(boxes, i + 1, right - 1, 0));
                }
            }
        }

        return dp[left][right][k];
    }
}
```

## 停在原地的方案数

> 1269
>
> *有一个长度为 arrLen 的数组，开始有一个指针在索引 0 处。*
>
> *每一步操作中，你可以将指针向左或向右移动 1 步，或者停在原地（指针不能被移动到数组范围外）。*
>
> *给你两个整数 steps 和 arrLen ，请你计算并返回：在恰好执行 steps 次操作以后，指针仍然指向索引 0 处的方案数。*
>
> *由于答案可能会很大，请返回方案数 模 10^9 + 7 后的结果。*

```java
class Solution {
    private static final int MOD = 10_0000_0007;

    public int numWays(int steps, int arrLen) {
        int maxDis = Math.min(arrLen - 1, steps);
        // dp[i][j]表示走了i步停在j处的方案数
        int[][] dp = new int[steps + 1][maxDis + 1];
        dp[0][0] = 1;

        for (int i = 1; i <= steps; i++) {
            for (int j = 0; j <= maxDis; j++) {
                dp[i][j] = dp[i - 1][j];
                if (j - 1 >= 0) {
                    dp[i][j] = (dp[i][j] + dp[i - 1][j - 1]) % MOD;
                }
                if (j + 1 <= maxDis) {
                    dp[i][j] = (dp[i][j] + dp[i - 1][j + 1]) % MOD;
                }
            }
        }

        return dp[steps][0];
    }
}
```

## 出界的路径数

> 576
>
> *给定一个 m × n 的网格和一个球。球的起始坐标为 (i,j) ，你可以将球移到相邻的单元格内，或者往上、下、左、右四个方向上移动使球穿过网格边界。*
>
> *但是，你最多可以移动 N 次。找出可以将球移出边界的路径数量。答案可能非常大，返回 结果 mod 109 + 7 的值。*

```java
// DFS 超时
class Solution {
    int res = 0;
    public int findPaths(int m, int n, int N, int i, int j) {
        dfs(m, n, N, i, j);
        return res;
    }
    public void dfs(int m, int n, int N, int i, int j){
        if(N == 0 && i >= 0 && i < m && j >= 0 && j < n) return;
        if(i >= N && m - i > N && j >= N && n - j > N) return;
        if(N >= 0 && (i == -1 || j == -1 || i == m || j == n)){
            res = (res + 1) % 1000000007;
            return;
        }
        dfs(m, n, N - 1, i + 1, j);
        dfs(m, n, N - 1, i - 1, j);
        dfs(m, n, N - 1, i, j + 1);
        dfs(m, n, N - 1, i, j - 1);
    }
}

// DP
class Solution {
    public int findPaths(int m, int n, int N, int i, int j) {
        if (N == 0) {return 0;}
        long[][][] dp = new long[m + 2][n + 2][N + 1];

        for (int r = 0; r <= m + 1; r++) {
            dp[r][0][0] = 1;
            dp[r][n + 1][0] = 1;
        }
        for (int c = 0; c <= n + 1; c++) {
            dp[0][c][0] = 1;
            dp[m + 1][c][0] = 1;
        }

        for (int k = 1; k <= N; k++) {
            for (int r = 1; r <= m; r++) {
                for (int c = 1; c <= n; c++) {
                    dp[r][c][k] = (dp[r - 1][c][k - 1] + dp[r + 1][c][k - 1] + dp[r][c - 1][k - 1] + dp[r][c + 1][k - 1]) % 1000000007;
                }
            }
        }

        int ans = 0;
        for (int k = 1; k <= N; k++) {
            ans = (int)((ans + dp[i + 1][j + 1][k]) % 1000000007);
        }
        return ans;
    }
}
```

## K个逆序对数组

> 629
>
> 给出两个整数 n 和 k，找出所有包含从 1 到 n 的数字，且恰好拥有 k 个逆序对的不同的数组的个数。
>
> 逆序对的定义如下：对于数组的第i个和第 j个元素，如果满i < j且 a[i] > a[j]，则其为一个逆序对；否则不是。
>
> 由于答案可能很大，只需要返回 答案 mod 109 + 7 的值

```java
public class Solution {
    public int kInversePairs(int n, int k) {
        int[][] dp = new int[n + 1][k + 1];
        int mod = 1000000007;
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= k && j <= i * (i - 1) / 2; j++) {
                if (i == 1 && j == 0) {
                    dp[i][j] = 1;
                    break;
                } else if (j == 0)
                    dp[i][j] = 1;
                else {
                    int val = (dp[i - 1][j] + mod - ((j - i) >= 0 ? dp[i - 1][j - i] : 0)) % mod;
                    dp[i][j] = (dp[i][j - 1] + val) % mod;
                }
            }
        }
        return dp[n][k];
    }
}
```

## 学生出勤记录

> *给定一个字符串来代表一个学生的出勤记录，这个记录仅包含以下三个字符：*
>
> *'A' : Absent，缺勤*
>
> *'L' : Late，迟到*
>
> *'P' : Present，到场*
>
> *如果一个学生的出勤记录中不超过一个'A'(缺勤)并且不超过两个连续的'L'(迟到),那么这个学生会被奖赏。*
>
> *你需要根据这个学生的出勤记录判断他是否会被奖赏。*

```java
class Solution {
    public boolean checkRecord(String s) {
        int countA = 0;
        int preL = 0;

        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == 'A') {
                countA++;
                if (countA > 1) {return false;}
            }
            else if (s.charAt(i) == 'L') {
                if (preL == i - 1 && (i > 1 && s.charAt(i - 2) == 'L')) {return false;}
                preL = i;
            }
        }

        return true;
    }
}
```

> *给定一个正整数 n，返回长度为 n 的所有可被视为可奖励的出勤记录的数量。 答案可能非常大，你只需返回结果mod 109 + 7的值。*
>
> *学生出勤记录是只包含以下三个字符的字符串：*
>
> *'A' : Absent，缺勤*
>
> *'L' : Late，迟到*
>
> *'P' : Present，到场*
>
> *如果记录不包含多于一个'A'（缺勤）或超过两个连续的'L'（迟到），则该记录被视为可奖励的。*

```java
class Solution {
    private static final long MOD = 1000000007;

    public int checkRecord(int n) {
        long[] dp = new long[n <= 5 ? 6 : n + 1];
        dp[0] = 1;
        dp[1] = 2;
        dp[2] = 4;
        dp[3] = 7;
        
        for (int i = 4; i <= n; i++) {
            dp[i] = ((2 * dp[i - 1]) % MOD + (MOD - dp[i - 4])) % MOD;
        }

        long sum = dp[n];
        for (int i = 1; i <= n; i++) {
            sum += (dp[i - 1] * dp[n - i]) % MOD;
        }

        return (int)(sum % MOD);
    }
}
```

## 矩形区域不超过 K 的最大数值和

> 363
>
> *给定一个非空二维矩阵 matrix 和一个整数 k，找到这个矩阵内部不大于 k 的最大矩形和。*

```java
class Solution {
    public int maxSumSubmatrix(int[][] matrix, int k) {
        int m = matrix.length, n = matrix[0].length;
        int ans = Integer.MIN_VALUE;
        
        for (int i1 = 1; i1 <= m; i1++) {
            for (int j1 = 1; j1 <= n; j1++) {
                int[][] dp = new int[m + 1][n + 1];
                dp[i1][j1] = matrix[i1 - 1][j1 - 1];
                
                for (int i2 = i1; i2 <= m; i2++) {
                    for (int j2 = j1; j2 <= n; j2++) {
                        dp[i2][j2] = dp[i2 - 1][j2] + dp[i2][j2 - 1] - dp[i2 - 1][j2 - 1] + matrix[i2 - 1][j2 - 1];
                        if (dp[i2][j2] <= k && dp[i2][j2] > ans) {
                            ans = dp[i2][j2];
                        }
                    }
                }
            }
        }
        return ans;
    }
}
```

## 最后一块石头的重量2

> 1049
>
> 有一堆石头，用整数数组 stones 表示。其中 stones[i] 表示第 i 块石头的重量。
>
> 每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：
>
> 如果 x == y，那么两块石头都会被完全粉碎；
> 如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
> 最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。

```java
// 要使最后一块石头的重量尽可能地小，neg需要在不超过sum/2的前提下尽可能地大。因此本问题可以看作是背包容量为sum/2，物品重量和价值均为stonesi的0-1背包问题。
// 对于该问题，定义二维布尔数组dp，其中dp[i+1][j]表示前i个石头能否凑出重量j。
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = Arrays.stream(stones).sum();
        int n = stones.length, m = sum / 2;
        boolean[][] dp = new boolean[n + 1][m + 1];
        dp[0][0] = true;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j <= m; j++) {
                if (j < stones[i]) {dp[i + 1][j] = dp[i][j];}
                else {dp[i + 1][j] = dp[i][j] || dp[i][j - stones[i]];}
            }
        }

        for (int j = m; j >= 0; j--) {
            if (dp[n][j]) {return sum - 2 * j;}
        }

        return 0;
    }
}
```

## 我能赢吗

> 464
>
> 在 "100 game" 这个游戏中，两名玩家轮流选择从 1 到 10 的任意整数，累计整数和，先使得累计整数和达到或超过 100 的玩家，即为胜者。
>
> 如果我们将游戏规则改为 “玩家不能重复使用整数” 呢？
>
> 例如，两个玩家可以轮流从公共整数池中抽取从 1 到 15 的整数（不放回），直到累计整数和 >= 100。

```java
class Solution {
    public boolean canIWin(int maxChoosableInteger, int desiredTotal) {
        if ((1 + maxChoosableInteger) * maxChoosableInteger / 2 < desiredTotal) {return false;}
        return myMethod(maxChoosableInteger, desiredTotal, new Boolean[1 << maxChoosableInteger], 0);
    }

    private boolean myMethod(int maxChoosableInteger, int desiredTotal, Boolean[] dp, int state) {
        if (dp[state] != null) {return dp[state];}

        for (int i = 1; i <= maxChoosableInteger; i++) {
            int cur = 1 << (i - 1);
            if ((cur & state) != 0) {continue;}

            if (desiredTotal - i <= 0 || myMethod(maxChoosableInteger, desiredTotal - i, dp, state | cur) == false){
                return dp[state] = true;
            }
        }

        return dp[state] = false;
    }
}
```

## 完成所有工作的最短时间

> 1723
>
> *给你一个整数数组 jobs ，其中 jobs[i] 是完成第 i 项工作要花费的时间。*
>
> *请你将这些工作分配给 k 位工人。所有工作都应该分配给工人，且每项工作只能分配给一位工人。*
>
> *工人的 工作时间 是完成分配给他们的所有工作花费时间的总和。*
>
> *请你设计一套最佳的工作分配方案，使工人的 最大工作时间 得以 最小化 。*
>
> 
>
> *返回分配方案中尽可能 最小 的 最大工作时间 。*

```java
class Solution {
    public int minimumTimeRequired(int[] jobs, int k) {
        int n = jobs.length;
        // 用一个 n 位的二进制整数来表示哪些工作已经被分配，哪些工作尚未被分配
        int[] sum = new int[1 << n];
        for (int i = 1; i < (1 << n); i++) {
            int x = Integer.numberOfTrailingZeros(i), y = i - (1 << x);
            sum[i] = sum[y] + jobs[x];
        }

        int[][] dp = new int[k][1 << n];
        for (int i = 0; i < (1 << n); i++) {dp[0][i] = sum[i];}

        for (int i = 1; i < k; i++) {
            for (int j = 0; j < (1 << n); j++) {
                int minN = Integer.MAX_VALUE;
                for (int x = j; x != 0; x = (x - 1) & j) {
                    minN = Math.min(minN, Math.max(dp[i - 1][j - x], sum[x]));
                }
                dp[i][j] = minN;
            }
        }

        return dp[k - 1][(1 << n) - 1];
    }
}
```

## 粉刷房子

> 1473
>
> *我们将连续相同颜色尽可能多的房子称为一个街区。（比方说 houses = [1,2,2,3,3,2,1,1] ，它包含 5 个街区 [{1}, {2,2}, {3,3}, {2}, {1,1}] 。）*
>
> *给你一个数组 houses ，一个 m \* n 的矩阵 cost 和一个整数 target ，其中：*
>
> *houses[i]：是第 i 个房子的颜色，0 表示这个房子还没有被涂色。*
>
> *cost[i][j]：是将第 i 个房子涂成颜色 j+1 的花费。*
>
> *请你返回房子涂色方案的最小总花费，使得每个房子都被涂色后，恰好组成 target 个街区。如果没有可用的涂色方案，请返回 -1 。*

```java
class Solution {
    // 极大值
    // 选择 Integer.MAX_VALUE / 2 的原因是防止整数相加溢出
    static final int INFTY = Integer.MAX_VALUE / 2;

    public int minCost(int[] houses, int[][] cost, int m, int n, int target) {
        // 将颜色调整为从 0 开始编号，没有被涂色标记为 -1
        for (int i = 0; i < m; ++i) {
            --houses[i];
        }

        // dp 所有元素初始化为极大值
        int[][][] dp = new int[m][n][target];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                Arrays.fill(dp[i][j], INFTY);
            }
        }

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (houses[i] != -1 && houses[i] != j) {
                    continue;
                }
                
                for (int k = 0; k < target; ++k) {
                    for (int j0 = 0; j0 < n; ++j0) {
                        if (j == j0) {
                            if (i == 0) {
                                if (k == 0) {
                                    dp[i][j][k] = 0;
                                }
                            } else {
                                dp[i][j][k] = Math.min(dp[i][j][k], dp[i - 1][j][k]);
                            }
                        } else if (i > 0 && k > 0) {
                            dp[i][j][k] = Math.min(dp[i][j][k], dp[i - 1][j0][k - 1]);
                        }
                    }

                    if (dp[i][j][k] != INFTY && houses[i] == -1) {
                        dp[i][j][k] += cost[i][j];
                    }
                }
            }
        }

        int ans = INFTY;
        for (int j = 0; j < n; ++j) {
            ans = Math.min(ans, dp[m - 1][j][target - 1]);
        }
        return ans == INFTY ? -1 : ans;
    }
}
```

## 多米诺和托米诺平铺

> 790
>
> *有两种形状的瓷砖：一种是 2x1 的多米诺形，另一种是形如 "L" 的托米诺形。两种形状都可以旋转。*
>
> *XX  <- 多米诺*
>
> *XX  <- "L" 托米诺*
>
> *X*
>
> *给定 N 的值，有多少种方法可以平铺 2 x N 的面板？返回值 mod 10^9 + 7。*
>
> *（平铺指的是每个正方形都必须有瓷砖覆盖。两个平铺不同，当且仅当面板上有四个方向上的相邻单元中的两个，使得恰好有一个平铺有一个瓷砖占据两个正方形。）*

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int numTilings(int n) {
        long[] dp = new long[]{1, 0, 0, 0};
        for (int i = 0; i < n; i++) {
            long[] ndp = new long[4];
            ndp[0] = (dp[0] + dp[3]) % MOD;
            ndp[1] = (dp[0] + dp[2]) % MOD;
            ndp[2] = (dp[0] + dp[1]) % MOD;
            ndp[3] = (dp[0] + dp[1] + dp[2]) % MOD;
            dp = ndp;
        }
        return (int)dp[0];
    }
}
```

## 给N*3网格图涂色

> 1411
>
> *你有一个 n x 3 的网格图 grid ，你需要用 红，黄，绿 三种颜色之一给每一个格子上色，且确保相邻格子颜色不同*
>
> *（也就是有相同水平边或者垂直边的格子颜色不同）。*
>
> *给你网格图的行数 n 。*
>
> *请你返回给 grid 涂色的方案数。由于答案可能会非常大，请你返回答案对 10^9 + 7 取余的结果。*

```java
class Solution {
    public int numOfWays(int n) {
        long dp2 = 6, dp3 = 6, mod = 1000000007;
        for(int i = 1; i < n; i++) {
            long temp2 = dp2;
            long temp3 = dp3;
            // 若当前使用 2 种颜色组成网格
            // 如果上一个网格是 2 种颜色，则当前网格有 3 种可能
            // 如果上一个网格是 3 种颜色，则当前网格有 2 种可能
            dp2 = (temp2 * 3 % mod + temp3 * 2 % mod) % mod;
            // 若当前使用 3 种颜色组成网格
            // 如果上一个网格是 2 种颜色，则当前网格有 2 种可能
            // 如果上一个网格是 3 种颜色，则当前网格有 2 种可能
            dp3 = (temp2 * 2 % mod + temp3 * 2 % mod) % mod;
        }
        return (int)((dp2 + dp3) % mod);
    }
}
```

## 最大加号标志

> 764
>
> *在一个大小在 (0, 0) 到 (N-1, N-1) 的2D网格 grid 中，除了在 mines 中给出的单元为 0，其他每个单元都是 1。*
>
> *网格中包含 1 的最大的轴对齐加号标志是多少阶？返回加号标志的阶数。如果未找到加号标志，则返回 0。*
>
> 
>
> *一个 k" 阶由 1 组成的“轴对称”加号标志具有中心网格 grid[x][y] = 1 ，*
>
> *以及4个从中心向上、向下、向左、向右延伸，长度为 k-1，由 1 组成的臂。下面给出 k" 阶“轴对称”加号标志的示例。*
>
> *注意，只有加号标志的所有网格要求为 1，别的网格可能为 0 也可能为 1。*

```java
class Solution {
    public int orderOfLargestPlusSign(int n, int[][] mines) {
        Set<Integer> ban = new HashSet<>();
        for (int[] cur : mines) {ban.add(cur[0] * n + cur[1]);}

        int[][] dp = new int[n][n];
        int ans = 0, count;

        for (int r = 0; r < n; r++) {
            count = 0;
            for (int c = 0; c < n; c++) {
                count = ban.contains(r * n + c) ? 0 : count + 1;
                dp[r][c] = count;
            }

            count = 0;
            for (int c = n - 1; c >= 0; c--) {
                count = ban.contains(r * n + c) ? 0 : count + 1;
                dp[r][c] = Math.min(dp[r][c], count);
            }
        }

        for (int c = 0; c < n; c++) {
            count = 0;
            for (int r = 0; r < n; r++) {
                count = ban.contains(r * n + c) ? 0 : count + 1;
                dp[r][c] = Math.min(dp[r][c], count);
            }

            count = 0;
            for (int r = n - 1; r >= 0; r--) {
                count = ban.contains(r * n + c) ? 0 : count + 1;
                dp[r][c] = Math.min(dp[r][c], count);
                ans = Math.max(ans, dp[r][c]);
            }
        }

        return ans;
    }
}
```
