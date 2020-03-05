# [621][Task Scheduler](https://leetcode.com/problems/task-scheduler/)
*Top100*
## 思路:
任务调度: 高频任务决定完整周期, 低频穿插在高频间隔里.
#### 1. 频率统计 + 数学
> **方法一:**(时间击败99%)<br>
***核心点***<br>
对单字符的重复次数进行统计, 因为限制了范围, 所以创建[26]长度的数组进行统计并排序. 假设最高频
次数为**10**, 这10次任务之间有9个间隔, 所以我们分成9个任务块, 每个任务块里包含一个高频任务,
以及**n**个坑(坑里用来插入低频任务), 此时就可以计算出来总的interval数目了(数学公式而已). **注意:** 如果
所有坑都填完了, 还有更低频的任务没安排, 那其实就可以理解为, 随便选一个块, 扩展一个坑用来填即可.
**因为, 最高频任务决定了冷却间隔, 低频任务不用考虑冷却间隔.** 所以最终结果: **Max(原字符数组本身的长度, 上面计算出来的结果)**<br>
***Java实现与逻辑分析***<br>
这里只再说明下那个数学公式, 假设"AAAABBB", n=2, 高频次数为4.<br>
A_ _ A_ _ A_ _ A, 有3个块, 每个块都有2个坑, 我们随便把B填进去, 一个块一个B, 长度公式为:<br>
(4-1)*(2+1)+1<br>
代码如下:<br>
```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        int len = tasks.length;
        int[] freq = new int[26];
        for(char c : tasks)
            freq[c-'A']++;
        Arrays.sort(freq);
        int i = 25, mx=freq[25];
        while(i>=0 && freq[i]==mx) i--;
        return Math.max(len, (mx-1)*(n+1)+25-i);
    }
}
```
> **方法二:**(时间击败50%)<br>
***核心点***<br>
这里只测试了下用TreeMap来帮忙统计频率, 但是, 容易统计每个字符(key)的个数, 但是对个数(value)排序却无能为力, 因此又转化成List进行排序, 导致效率不如方法一. 算做是复习吧.
内容<br>
***Java实现与逻辑分析***   <br>
内容:<br>代码如下:
```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        Map<Character, Integer> tmap = new TreeMap<>();
        for(char c : tasks) {
            if(tmap.get(c)==null) tmap.put(c, 1);
            else tmap.put(c, tmap.get(c)+1);
        }
        List<Map.Entry<Character, Integer>> list = new ArrayList<>(tmap.entrySet());
        Collections.sort(list, new Comparator<Map.Entry<Character, Integer>>(){
           @Override
            public int compare(Map.Entry<Character, Integer> o1, Map.Entry<Character, Integer> o2) {
                return o1.getValue().compareTo(o2.getValue());
            }
        });
        int length = list.size()-1;
        int i = length, mx = list.get(length).getValue();
        while(i>=0 && list.get(i).getValue() == mx) i--;
        return Math.max(tasks.length, (mx-1)*(n+1)+length-i);

    }
}
```
