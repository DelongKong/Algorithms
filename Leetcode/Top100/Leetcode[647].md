# [647][Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)
*Top100*
## 思路:
计算回文子串个数
#### 1. 单层遍历 + 递归 + 回文对称特性
> **方法一:**<br>
***核心点***<br>
判断一个回文序列, 从中间数(对称轴)开始往两侧延展, 这样得话在此基础上扩展的序列就不需要重复计算是否相等. 注意, 回文序列长度在奇偶情况下, 对称轴不一样, 奇数个, 中间值就是唯一对称轴, 偶数个, 我们直接把中间两个数判断并作为对称轴.<br>
***Java实现与逻辑分析***<br>
从第一个元素开始遍历, 每个元素进行两次递归, 分别是 **Recursive(s, i, i)** 和 **Recursive(s, i, i+1)**, i是当前位置索引. 递归函数返回回文序列的个数.
代码如下:<br>
```java
class Solution {
    public int countSubstrings(String s) {
        if(s.length() == 0) return 0;
        int count=0;
        for(int i = 0; i < s.length()-1; i++) {
            count += (countsubstrings(s, i, i) + countsubstrings(s, i, i+1));
        }
        return count+1;
    }
    public int countsubstrings(String s, int left, int right) {
        int count=0;
        while(left>=0 && right < s.length() && s.charAt(left)==s.charAt(right)) {
            count++;
            left--;
            right++;
        }
        return count;
    }
}
```
#### 2. 动态规划
> **方法一:** <br>
***核心点***<br>
***Java实现与逻辑分析***<br>
代码如下:
```java

```
> **方法二:**<br>
**核心点**<br>
**Java实现与逻辑分析**<br>
代码如下:
```java

```
