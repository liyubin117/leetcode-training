# day11

## 第十一天| 20 1047 150

### 20 有效的括号 easy
```
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
每个右括号都有一个对应的相同类型的左括号。
```
https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html

思路：用栈

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (Character c: s.toCharArray()) {
            if (c == '(' || c == '[' || c == '{') stack.push(c);
            else if (c == ')' || c == ']' || c == '}') {
                if (stack.isEmpty()) return false;
                Character top = stack.pop();
                if (c == ')' && top != '(') return false;
                if (c == ']' && top != '[') return false;
                if (c == '}' && top != '{') return false;
            }
        }
        return stack.isEmpty();
    }
}
```

### 1047 删除字符串中的所有相邻重复项 easy
```
给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。
```
https://programmercarl.com/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.html

思路：用堆，为了不倒序生成字符串，用ArrayDeque这种可双端操作的栈结构

```java
class Solution {
    public String removeDuplicates(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        for (Character c: s.toCharArray()) {
            if (stack.isEmpty() || stack.peek() != c) stack.push(c);
            else if(stack.peek() == c) stack.pop();
        }
        StringBuffer sb = new StringBuffer();
        while (!stack.isEmpty()) {
            sb.append(stack.pollLast());
        }
        return sb.toString();
    }
}
```

### 150 逆波兰表达式求值 middle
```
给你一个字符串数组 tokens ，表示一个根据 逆波兰表示法 表示的算术表达式。

请你计算该表达式。返回一个表示表达式值的整数。

注意：

有效的算符为 '+'、'-'、'*' 和 '/' 。
每个操作数（运算对象）都可以是一个整数或者另一个表达式。
两个整数之间的除法总是 向零截断 。
表达式中不含除零运算。
输入是一个根据逆波兰表示法表示的算术表达式。
答案及所有中间计算结果可以用 32 位 整数表示。
```
https://programmercarl.com/0150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.html

思路：用栈，每次遇到非表达式时就加到栈，遇到表达式时就弹出前两个作为x和y，再根据当前表达式进行计算后写入栈

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<String> stack = new ArrayDeque<>();
        Map<String, BiFunction<Integer, Integer, Integer>> map = new HashMap<>();
        map.put("+", (x, y) -> x + y);
        map.put("-", (x, y) -> x - y);
        map.put("*", (x, y) -> x * y);
        map.put("/", (x, y) -> x / y);
        for (String token : tokens) {
            if (!map.containsKey(token)) stack.push(token);
            else {
                Integer y = Integer.valueOf(stack.pop());
                Integer x = Integer.valueOf(stack.pop());
                stack.push(String.valueOf(map.get(token).apply(x, y)));
            }
        }
        return Integer.valueOf(stack.pop());
    }
}
```