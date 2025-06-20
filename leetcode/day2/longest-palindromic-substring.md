# LeetCode 5. 最长回文子串

## 🧩 题目描述

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

**示例 1：**
```
输入: s = "babad"
输出: "bab"
解释: "aba" 也是正确答案。
```

**示例 2：**
```
输入: s = "cbbd"
输出: "bb"
```

**提示：**
- `1 <= s.length <= 1000`
- `s` 仅由数字和英文字母组成

---

## 💡 解法：中心扩展法（Java 实现）

### ✅ 核心思想：
每一个字符都可能是一个回文的中心。枚举每个位置 i：
- 从 i 向左右两边扩展，看能否构成回文（奇数长度）
- 还要检查 i 和 i+1 之间是否能构成偶数长度的回文（偶数长度）

---

## ✅ Java 实现代码（含详细注释）

```java
public class LongestPalindrome {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) return "";

        int start = 0, end = 0;

        for (int i = 0; i < s.length(); i++) {
            // 以当前字符为中心扩展（奇数长度）
            int len1 = expandAroundCenter(s, i, i);

            // 以当前字符和下一个字符之间为中心扩展（偶数长度）
            int len2 = expandAroundCenter(s, i, i + 1);

            // 选出最长的回文长度
            int len = Math.max(len1, len2);

            // 如果找到了更长的回文子串，更新起始与结束位置
            if (len > end - start) {
                // 起始点：从中心 i 往左扩展 (len - 1)/2 个单位
                start = i - (len - 1) / 2;

                // 结束点：从中心 i 往右扩展 len / 2 个单位
                end = i + len / 2;
            }
        }

        // 从 s 中截取最长回文子串
        return s.substring(start, end + 1);
    }

    // 从中心 left 和 right 向两边扩展，返回回文串长度
    private int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        // 注意：right-left-1 是回文串长度
        return right - left - 1;
    }
}
```

---

## 🤔 你提出的疑问讲解

### 🔸 `for (int i = 0; i < s.length(); i++)`
你写成了：
```java
for (int i = 0; i < s.length();, i++)
```
这里错误地用了逗号 `,`，Java 中需要用分号 `;` 来分隔三部分：初始化；条件；步进。

---

### 🔸 `start = i - (len - 1) / 2;` 和 `end = i + len / 2;` 是什么？

假设当前回文长度为 `len`，中心位置是 `i`，那我们需要：

- **起点位置 start：**
  向左偏移 `(len - 1) / 2`，因为：
  - 如果是奇数：比如 `len = 5`，中心是第 2 个字符，起点应该是 `2 - 2 = 0`
  - 如果是偶数：比如 `len = 4`，中心在中间两个字符之间，仍然起点是 `2 - 1 = 1`

- **终点位置 end：**
  向右偏移 `len / 2`，这样就能涵盖完整的子串。

---

## ✅ 示例执行过程演示

以 "babad" 为例：

| i  | 中心 | 回文串   | 长度 |
|----|------|----------|------|
| 0  | b    | b        | 1    |
| 1  | a    | bab      | 3    |
| 2  | b    | aba      | 3    |
| 3  | a    | a        | 1    |
| 4  | d    | d        | 1    |

返回 "bab" 或 "aba"

---

## 📌 推荐标签：
`#字符串` `#双指针` `#动态规划（可选）` `#中心扩展法`
