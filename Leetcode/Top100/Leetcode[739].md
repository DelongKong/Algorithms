# [739][Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
*Top(100/100)*
##思路:

总体上, 需要找到每个位置处有多长的升序序列, 如果没有, 标记为0.
#### 1.采用双循环遍历, 能做但是效率很差. 时间上O(N^2) 空间上O(1).

#### 2.倒序遍历 + 栈
> **方法一:**
**核心点** 栈如果想存放T[i]即数组的元素, 则需要同时记录下该元素的下标索引, 因为要统计升序序列的长度(本来最开始一直想只用数组元素进栈就解决问题, 但失败了, 即使成功也相当麻烦). 考虑用一个Pair<T[i], i>的结构来进行栈操作.
**Java实现与逻辑分析**
采用的是倒序遍历, 对于遍历的每一个数组元素T[i]:
(1) 如果小于栈顶元素, 把 **<T[i], i>** 压入栈, 此时, 可以直接记录该位置的升序序列长度为1, 即T[i] = 1 (不需要再开辟空间).
(2) 如果大于等于栈顶元素, 则通过while循环不断弹出栈顶元素, 直到找到比它大的栈顶元素, 停止, 计算长度=两者的下标差; 或者直到栈为空, 说明没有更大的气温, T[i]设置为0; 最后别忘了在设置T[i]前, 先把其自身压入栈里! 代码如下:
```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        Stack<Pair<Integer, Integer>> stack = new Stack<>();
        for(int i = T.length-1; i >= 0; i--) {
            while(!stack.empty() && T[i] >= (stack.peek().getKey()))
                stack.pop();
            if(stack.empty()) {
                stack.push(new Pair<Integer, Integer>(T[i], i));
                T[i] = 0;
            }else{
                int p = stack.peek().getValue();
                stack.push(new Pair<Integer, Integer>(T[i], i));
                T[i]= p - i;
            }
        }
        return T;
    }
}
```
> **方法二:**
**核心点** 栈直接存放数组T的下标索引. 这样做就可以很方便地计算升序序列的长度.
**Java实现与逻辑分析**
采用倒序遍历, 对于遍历的每一个数组元素T[i]:
(1) 如果小于栈顶元素, 把下标i压入栈
(2) 如果大于等于栈顶元素, 不断出栈, 直到小于某栈顶, 或者栈为空. 代码如下:
```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        Stack<Integer> stack = new Stack<>();
        int[] res = new int[T.length];
        for(int i = T.length-1; i >= 0; i--) {
            // pop from the stack
            while(!stack.empty() && T[i] >= T[stack.peek()]) stack.pop();
            // two situations: find bigger one or stack is empty
            if(stack.empty()) res[i] = 0;
            else res[i] = stack.peek() - i;
            stack.push(i);
        }
        return res;
    }
}
```
