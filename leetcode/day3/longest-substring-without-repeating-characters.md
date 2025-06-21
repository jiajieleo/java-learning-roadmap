# 第 3 天  —  无重复字符的最长子串（LeetCode 3）

*日期*：2025-06-21  
*题号*：LeetCode #3  
*难度*：中等  
*题目链接*：<https://leetcode.cn/problems/longest-substring-without-repeating-characters/>

---

## 1 题目描述  
给定一个字符串 `s`，请找出其中 **不含重复字符的最长子串** 的长度，并返回该长度。

示例 | 输入 | 输出  
:-- | :-- | :--  
示例 1 | `"abcabcbb"` | `3`（`"abc"`）  
示例 2 | `"bbbbb"`    | `1`（`"b"`）  
示例 3 | `"pwwkew"`   | `3`（`"wke"`）

---

## 2 暴力解（了解即可）  
1. 穷举所有子串 `O(n²)`  
2. 判断子串是否有重复 `O(n)`  
3. 总复杂度 `O(n³)` — 太慢。

---

## 3 最优思路：滑动窗口  
**核心词**：两根指针只向右移动，不回头。

| 变量 | 作用 |
| ---- | ---- |
| `left`  | 窗口左边界 |
| `right` | 窗口右边界，每轮 +1 |
| `window`| `HashSet`，保存窗口内字符 |
| `maxLen`| 记录遇到的最大窗口长度 |

步骤  
1. `right` 从 0 扫到尾  
2. 若当前字符 `c` 已在 `window` 中 → 不断 `left++` 并移出窗口直至无重复  
3. 将 `c` 加入 `window`  
4. 用 `Math.max` 刷新 `maxLen`

**时间复杂度 O(n)**（两个指针各走一次）。  
空间复杂度 O(Σ) ≈ 常量。

---

## 4 最终代码（已修正所有拼写）

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {

    public int lengthOfLongestSubstring(String s) {
        // 0️⃣ 特判：空串
        if (s == null || s.length() == 0) {
            return 0;
        }

        Set<Character> window = new HashSet<>(); // 1️⃣ 窗口
        int left   = 0;      // 左边界
        int maxLen = 0;      // 最长长度

        // 2️⃣ right 指针右移
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);           // 正确写法 charAt

            // 3️⃣ 出现重复 -> 缩小窗口
            while (window.contains(c)) {
                window.remove(s.charAt(left));  // 同样 charAt
                left++;
            }

            // 4️⃣ 加入新字符 更新答案
            window.add(c);
            maxLen = Math.max(maxLen, right - left + 1); // Math.max
        }
        return maxLen;
    }
}
```

---

## 5 常见疑问 + 你的报错

| 序号 | 疑问 / 报错 | 解释 / 解决 |
| ---- | ----------- | ----------- |
| 1 | `==` 与 `||`？ | `==` 比较相等；`||` 逻辑或，只要一侧为 `true` 整体就 `true`。 |
| 2 | `Set<Character>` 后面 `<>`？ | 泛型。左边 `<Character>` 指集合元素类型；右边 `<>` 是菱形语法，编译器自动推断。 |
| 3 | `s.length()` 的 `()`？ | 方法调用的空参数列表。 |
| 4 | `maxLen = Math.max(...)`？ | 每轮刷新当前最大长度。 |
| 5 | 报错 `cannot find symbol CharAr` | 因为应写 `charAt` 大小写拼错。 |
| 6 | 报错 `Max.max` | 应为 `Math.max`（Java 自带数学工具类）。 |
| 7 | 如何算复杂度？ | 两指针总共移动 ≤2n 次 → O(n)；窗口用哈希表查找 O(1)。 |

---

## 6 手动画表辅助理解  
> 以 `abcabcbb` 为例，画 `left`/`right` 移动过程，窗口内容随时更新，体会没有回头的“两指针”思想。

---

## 7 今日收获  
* 滑动窗口套路：**“窗口里永远满足条件”**  
* HashSet 在窗口判重中的 O(1) 作用  
* `cannot find symbol` 90% 都是拼写 / import  
* 时间复杂度分析要点：指针走几步、哈希查找常量级。

---

**任务**：把本文件放到 `leetcode/day3/` 并在 22:00 前 push 到 GitHub。
