# 剑指Offer题解

## 剑指 Offer 03. 数组中重复的数字

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

>   示例 1：
>
>   输入：
>
>   [2, 3, 1, 0, 2, 5, 3]
>
>   输出：2 或 3 
>
>
>   限制：
>
>   2 <= n <= 100000

```java
public int findRepeatNumber(int[] nums) {
    if (nums.length == 0 || nums == null) {
        return -1;
    }
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(nums[i])) {
            return nums[i];
        } else {
            map.put(nums[i], 1);
        }
    }
    return -1;
}
```

## 剑指 Offer 04. 二维数组中的查找

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

>   示例:
>
>   现有矩阵 matrix 如下：
>
>   ```
>   [
>     [1,   4,  7, 11, 15],
>     [2,   5,  8, 12, 19],
>     [3,   6,  9, 16, 22],
>     [10, 13, 14, 17, 24],
>     [18, 21, 23, 26, 30]
>   ]
>   ```
>
>   给定 target = 5，返回 true。
>
>   给定 target = 20，返回 false。
>
>   限制：
>
>   0 <= n <= 1000
>
>   0 <= m <= 1000

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if (matrix == null) {
            return false;
        }
        //从左下角开始
        int row = matrix.length - 1, column = 0;
        while (row >= 0 && column < matrix[0].length) {
            if (matrix[row][column] == target) {
                return true;
            } else if (matrix[row][column] > target){
                row--;
                continue;
            } else {
                column++;
            }
        }
        return false;
    }
}
```

## 剑指 Offer 05. 替换空格

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

>   示例 1：
>
>   输入：s = "We are happy."
>
>   输出："We%20are%20happy."
>
>
>   限制：
>
>   0 <= s 的长度 <= 10000
>

```java
class Solution {
    public String replaceSpace(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                sb.append("%20");
            } else {
                sb.append(c);
            }
        }
        return sb.toString();
    }
}
```

## 剑指 Offer 06. 从尾到头打印链表

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

>   示例 1：
>
>   输入：head = [1,3,2]
>
>   输出：[2,3,1]
>
>
>   限制：
>
>   0 <= 链表长度 <= 10000
>

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public int[] reversePrint(ListNode head) {
        if (head == null) {
            return new int[0];
        }
        ListNode p = head;
        Stack<Integer> stack = new Stack<>();
        int cnt = 0;
        while (p != null) {
            stack.push(p.val);
            p = p.next;
            cnt ++;
        }
        int[] res = new int[cnt];
        int num = 0;
        while (!stack.isEmpty()) {
            res[num++] = stack.pop();
        }
        return res;
    }
}
```

## 剑指 Offer 07. 重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

>   例如，给出
>
>   前序遍历 preorder = [3,9,20,15,7]
>
>   中序遍历 inorder = [9,3,15,20,7]
>
>   返回如下的二叉树：
>
>       	3
>          / \
>         9  20
>           /  \
>          15   7
>
>   限制：
>
>   0 <= 节点个数 <= 5000
>

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder.length == 0 || inorder.length == 0) {
            return null;
        }
        //根节点为前序序列中的第一个数
        TreeNode root = new TreeNode(preorder[0]);
        //遍历中序序列，找到当前头节点在中序序列中的位置
        for (int i = 0; i < inorder.length; i++) {
            if (inorder[i] == preorder[0]) {
                //递归构建左右子树
                //preorder[1~i]为左子树的前序序列, inorder[0~i-1]为左子树的中序序列
                root.left = buildTree(Arrays.copyOfRange(preorder, 1, i + 1),
                                      Arrays.copyOfRange(inorder, 0, i));
                //preorder[i+1~最后]为右子树的前序序列, inorder[i+1~最后]为右子树的中序序列
                root.right = buildTree(Arrays.copyOfRange(preorder, i + 1, preorder.length),
                                       Arrays.copyOfRange(inorder, i + 1, inorder.length));
                break;
            }
        }
        return root;
    }
}
```

## 剑指 Offer 09. 用两个栈实现队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 ) 

>   示例 1：
>
>   ```
>   输入：
>   ["CQueue","appendTail","deleteHead","deleteHead"]
>   [[],[3],[],[]]
>   输出：
>   [null,null,3,-1]
>   ```
>
>   示例 2：
>
>   ```
>   输入：
>   ["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
>   [[],[],[5],[2],[],[]]
>   输出：
>   [null,-1,null,null,5,2]
>   ```
>
>   提示：
>
>   1 <= values <= 10000
>
>   最多会对 appendTail、deleteHead 进行 10000 次调用

```java
class CQueue {
    Stack<Integer> stack1;
    Stack<Integer> stack2;

    public CQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }

    public void appendTail(int value) {
        stack1.push(value);
    }

    public int deleteHead() {
        if (stack2.isEmpty()) {
            if (stack1.isEmpty()) {
                return -1;
            }
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
        
    }
}
```

## 剑指 Offer 10- I. 斐波那契数列

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
```

斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

>   示例 1：
>
>   输入：n = 2
>
>   输出：1
>
>   示例 2：
>
>   输入：n = 5
>
>   输出：5
>
>
>   提示：
>
>   0 <= n <= 100
>

```java
class Solution {
    public int fib(int n) {
        if(n == 0)
            return 0;
        int[] result = new int[n + 1];
        result[0] = 0;
        result[1] = 1;
        for(int i = 2; i <= n; i++)
            result[i] = (result[i - 2] + result[i - 1])  % 1000000007;
        return result[n];
    }
}
```

## 剑指 Offer 10- II. 青蛙跳台阶问题

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

>   示例 1：
>
>   输入：n = 2
>
>   输出：2
>
>   示例 2：
>
>   输入：n = 7
>
>   输出：21
>
>   提示：
>
>   0 <= n <= 100
>

```java
class Solution {
    public int numWays(int n) {
        if (n <= 1) {
            return 1;
        }
        int[] result = new int[n + 1];
        result[1] = 1;
        result[2] = 2;
        for (int i = 3; i <= n; i ++) {
            result[i] = (result[i - 1] + result[i - 2]) % 1000000007;
        }
        return result[n];
    }
}
```

## 剑指 Offer 11. 旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

>   示例 1：
>
>   输入：[3,4,5,1,2]
>
>   输出：1
>
>   示例 2：
>
>   输入：[2,2,2,0,1]
>
>   输出：0

```java
class Solution {
    public int minArray(int[] numbers) {
        if (numbers.length == 0) {
            return 0;
        }
        for (int i = 1; i < numbers.length; i ++) {
            if (numbers[i] < numbers[i - 1]) {
                return numbers[i];
            }
        }
        return numbers[0];
    }
}
```

## 剑指 Offer 12. 矩阵中的路径

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

```
[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]
```

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

>   示例 1：
>
>   输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
>
>   输出：true
>
>   示例 2：
>
>   输入：board = [["a","b"],["c","d"]], word = "abcd"
>
>   输出：false
>
>   提示：
>
>   1 <= board.length <= 200
>
>   1 <= board[i].length <= 200

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        if (board == null || board.length == 0) {
            return false;
        }
        char[] chars = word.toCharArray();
        // 依次遍历二维矩阵的所有节点
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (dfs(board, chars, i, j, 0)) {
                    // 剪枝，已经找到，没有必要再往下继续
                    return true;
                }
            }
        }
        return false;
    }

    public boolean dfs(char[][] board, char[] words, int line, int row, int index) {
        // 剪枝
        if (line >= board.length 
        || row >= board[0].length 
        || line < 0 
        || row < 0 
        || board[line][row] != words[index]) {
            return false;
        }
        // 单词的最后一个字母也匹配上了,整体匹配完成
        if (index == words.length - 1) {
            return true;
        }
        // 已经匹配上的字母不能重复匹配，暂时修改节点的值
        char temp = board[line][row];
        board[line][row] = '/';
        boolean ifExist = dfs(board, words, line + 1, row, index + 1) 
                        || dfs(board, words, line - 1, row, index + 1)
                        || dfs(board, words, line, row + 1, index + 1) 
                        || dfs(board, words, line, row - 1, index + 1);
        // 还原节点上的值，以免影响下面的匹配
        board[line][row] = temp;
        // 返回这个节点开始的整体匹配结果 
        return ifExist;
    }
}
```

