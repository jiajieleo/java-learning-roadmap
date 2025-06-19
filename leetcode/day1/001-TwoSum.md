# LeetCode 001 - 两数之和（Two Sum）

## 📌 题目描述
给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的两个整数，并返回它们的数组下标。

你可以假设每种输入只对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

## 🧠 解法一：暴力双层遍历
- 时间复杂度：O(n²)
- 空间复杂度：O(1)

## 🧠 解法二：哈希表查找差值（推荐）
- 时间复杂度：O(n)
- 空间复杂度：O(n)

### ✅ Java 代码（含中文注释）

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int[] twoSum(int[] nums, int target) {
        // 创建一个哈希表，key 为数值，value 为下标
        Map<Integer, Integer> map = new HashMap<>();

        // 遍历数组
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i]; // 计算目标值与当前值的差

            // 如果差值在哈希表中，说明找到了两个数
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i }; // 返回下标
            }

            // 如果没有，记录当前值和下标
            map.put(nums[i], i);
        }

        // 如果没找到，返回空数组
        return new int[] {};
    }
}
```

## 💡 我的理解
- 使用哈希表存储“数值 -> 下标”的映射
- 每次遍历时，检查目标值与当前值的差值是否已存在
- 一次遍历就能解决问题，效率高，适合大数据量
