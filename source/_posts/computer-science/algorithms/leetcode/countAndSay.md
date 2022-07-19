---
title: 外观数列
date: 2022-03-01 13:00:14
categories:
 - [计算机科学,数据结构与算法,leetcode]
tags: 
 - 数据结构
 - 算法
 - 编程技术
 - leetcode
 - 外观数列
---

# 求解外观数列
给定一个正整数n(1 <= n <= 30), 输出外观数列的第n项。
注意：整数序列中的每一项将表示为一个字符串。
[外观数列]是一个整数数列，从数字一开始，序列中的每一项都是对前一项的描述。前五项如下：
:::default no-icon
- 1. 1
- 2. 11
- 3. 21
- 4. 1211
- 5. 111221
:::

第一项是数字1
描述前一项，这个数是1即“一个1”，记作11
描述前一项，这个数是11即“两个1”，记作21
描述前一项，这个数是21即“一个2一个1”，记作1211
描述前一项，这个数是1211即“一个1一个2两个1”，记作111221

+++ 示例1：
输入：1
输出：“1”
解释：这是一个基本样例
+++
+++
示例2：
输入：4
输出：“1211”
解释：当n = 3时，序列是“21”，其中我们有“2”和“1”两组，“2”可以读作“12”，也就是频次 = 1而值 = 2；类似“1”可以对坐“11”。所以答案是“12”和“11”组合在一起，也就是“1211”
+++

## 分析
```java
class Solution{
    public String countAndSay(int n){
        if(n == 1){
            return "1";
        }
        String lastStr = countAndSay(n-1);
        StringBuilder ans = new StringBuilder();
        int i = 0, j = 1, len = lastStr.length();
        while(j < len){
            if(lastStr.charAt(i) != lastStr.charAt(j)){
                ans.append(j - i).append(lastStr.charAt(i));
                i = j;
            }
            j++;
        }
        ans.append(j - i).append(lastStr.charAt(i));
        return ans.toString();
    }
}
```
