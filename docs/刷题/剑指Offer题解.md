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

## 剑指 Offer 13. 机器人的运动范围

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

>   示例 1：
>
>   输入：m = 2, n = 3, k = 1
>
>   输出：3
>
>   示例 2：
>
>   输入：m = 3, n = 1, k = 0
>
>   输出：1
>
>   提示：
>
>   1 <= n,m <= 100
>   0 <= k <= 20

```java
class Solution {
    //统计到达的数量
    int counts = 0;
    public int movingCount(int m, int n, int k) {
        //辅助数组 用来标记是否统计过
        int[][] visited = new int[m][n];
        //从 0,0位置开始统计
        helper(visited, 0, 0, m - 1, n - 1, k);
        return counts;
    }
    /**
    *传入i,j两点 判断当前点是否符合规则 符合规则下继续对向下向右两个方向递归判断
    */
    private void helper(int[][] visited, int i, int j, int m, int n, int k){
        if(i<=m && j<=n && visited[i][j]!=1 && (indexSum(i)+indexSum(j))<=k){
            counts++;
            visited[i][j]=1;
            helper(visited, i + 1, j, m, n, k);
            helper(visited, i, j + 1, m, n, k);
        }
    }
    /**
    *根据传入的数 求出各位上的数字累加和
    */
    private int indexSum(int index){
        int sum = 0;
        while(index > 0) {
            int temp = index % 10;
            sum += temp;
            index = index / 10;
        }
        return sum;
    }
}
```

## 剑指 Offer 14- I. 剪绳子

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

>   示例 1：
>
>   输入: 2
>
>   输出: 1
>
>   解释: 2 = 1 + 1, 1 × 1 = 1
>
>   示例 2:
>
>   输入: 10
>
>   输出: 36
>
>   解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
>
>   提示：
>
>   2 <= n <= 58

```java
/**
* 解题思路，找出最优解的规律
* 当target等于1，2，3的时候，结果是固定的
* 当target大于3的时候，可以看以下数据
* target=4, 最优解：2 2
* target=5, 最优解：3 2
* target=6, 最优解：3 3
* target=7, 最优解：3 2 2
* target=8, 最优解：3 3 2
* target=9, 最优解：3 3 3
* target=10,最优解：3 3 2 2
* target=11,最优解：3 3 3 2
* target=12,最优解：3 3 3 3
* target=13,最优解：3 3 3 2 2
* target=14,最优解：3 3 3 3 2
* target=15,最优解：3 3 3 3 3
* 所以不难发现3和2的个数规律
*/
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3) return n-1;
        //记录数字总个数
        int length = 0;
        //记录数字2的个数
        int length2 = 0;
        if (n % 3 == 0) {
            length = n / 3;
            length2 = 0;
        } else {
            length = n / 3 + 1;
            length2 = 3 - n % 3;
        }
        int result = 1;
        //算乘积
        for(int i = 0; i < length; i++){
            result = result * ((i < length - length2) ? 3 : 2);
        }
        return result;
    }
}
```

## 剑指 Offer 14- II. 剪绳子 II

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m - 1] 。请问 k[0]*k[1]*...*k[m - 1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

**答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。**

>   示例 1：
>
>   输入: 2
>
>   输出: 1
>
>   解释: 2 = 1 + 1, 1 × 1 = 1
>
>   示例 2:
>
>   输入: 10
>
>   输出: 36
>
>   解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
>
>
>   提示：
>
>   **2 <= n <= 1000**

```java
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3) return n-1;
        long res = 1;
        while(n > 4){
            res *= 3;
            res = res % 1000000007;
            n -= 3;
        }
        return (int)(res *n % 1000000007);
    }
}
```

## 剑指 Offer 15. 二进制中1的个数

请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

>   示例 1：
>
>   输入：00000000000000000000000000001011
>   输出：3
>   解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
>   示例 2：
>
>   输入：00000000000000000000000010000000
>   输出：1
>   解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
>   示例 3：
>
>   输入：11111111111111111111111111111101
>   输出：31
>   解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。

```java
public class Solution {
    public int hammingWeight(int n) {
        int count = 0;
        while(n != 0){
            count++;
            n = n & (n - 1);
        }
        return count;
    }
}
```

## 剑指 Offer 16. 数值的整数次方

实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

>   示例 1:
>
>   输入: 2.00000, 10
>
>   输出: 1024.00000
>
>   示例 2:
>
>   输入: 2.10000, 3
>
>   输出: 9.26100
>
>   示例 3:
>
>   输入: 2.00000, -2
>
>   输出: 0.25000
>
>   解释: 2^-2^ = 1/2^2^ = 1/4 = 0.25
>
>
>   说明:
>
>   -100.0 < x < 100.0
>
>   n 是 32 位有符号整数，其数值范围是 [−2^31^, 2^31^ − 1] 。

```java
class Solution {
    public double myPow(double x, int n) {
        if(x == 0)  return 0;
        long b = n;
        double result = 1.0;
        if(b < 0){
            x = 1 / x;
            b = -b;
        }
        while(b > 0){
            if((b & 1) == 1) {
                result *= x;
            } 
            x *= x;
            b /= 2;
        }
        return result;
    }
}
```

## 剑指 Offer 17. 打印从1到最大的n位数

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

>   示例 1:
>
>   输入: n = 1
>
>   输出: [1,2,3,4,5,6,7,8,9]
>
>
>   说明：
>
>   用返回一个整数列表来代替打印
>
>   n 为正整数

```java
class Solution {
    public int[] printNumbers(int n) {
        if(n < 1) return null;
        //求最大值，10的n次方
        int max = 1;
        while(n >= 1){
            max *= 10;
            n--;
        }
        int[] ret = new int[max - 1];
        for(int i = 0; i < max - 1; i++){
            ret[i] = i + 1;
        }
        return ret;
    }
}
```

## 剑指 Offer 18. 删除链表的节点

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

>   **示例 1:**
>
>   ```
>   输入: head = [4,5,1,9], val = 5
>   输出: [4,1,9]
>   解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
>   ```
>
>   **示例 2:**
>
>   ```
>   输入: head = [4,5,1,9], val = 1
>   输出: [4,5,9]
>   解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
>   ```

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
    public ListNode deleteNode(ListNode head, int val) {
        if (head == null || head.next == null) {
            return null;
        }
        if (head.val == val) {
            return head.next;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null) {
            if (fast.val == val) {
                slow.next = fast.next;
                break;
            }
            fast = fast.next;
            slow = slow.next;
        }
        return head;
    }
}
```

## 剑指 Offer 19. 正则表达式匹配

请实现一个函数用来匹配包括`'.'`和`'*'`的正则表达式。模式中的字符``'.'``表示任意一个字符，而``'*'``表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但是与`"aa.a"`和`"ab*a"`均不匹配

```java
class Solution {
    public boolean isMatch(String A, String B) {
        int n = A.length();
        int m = B.length();
        boolean[][] f = new boolean[n + 1][m + 1];

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                //分成空正则和非空正则两种
                if (j == 0) {
                    f[i][j] = i == 0;
                } else {
                    //非空正则分为两种情况 * 和 非*
                    if (B.charAt(j - 1) != '*') {
                        if (i > 0 && (A.charAt(i - 1) == B.charAt(j - 1) 
                                      || B.charAt(j - 1) == '.')) {
                            f[i][j] = f[i - 1][j - 1];
                        }
                    } else {
                        //碰到 * 了，分为看和不看两种情况
                        //不看
                        if (j >= 2) {
                            f[i][j] |= f[i][j - 2];
                        }
                        //看
                        if (i >= 1 && j >= 2 && (A.charAt(i - 1) == B.charAt(j - 2) 
                                                 || B.charAt(j - 2) == '.')) {
                            f[i][j] |= f[i - 1][j];
                        }
                    }
                }
            }
        }
        return f[n][m];
    }
}
```

## 剑指 Offer 20. 表示数值的字符串

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"、"-1E-16"及"12e+5.4"都不是。

```java
class Solution {
    public boolean isNumber(String s) {
        if(s == null || s.length() == 0){
            return false;
        }
        //标记是否遇到相应情况
        boolean numSeen = false;
        boolean dotSeen = false;
        boolean eSeen = false;
        char[] str = s.trim().toCharArray();
        for(int i = 0;i < str.length; i++){
            if(str[i] >= '0' && str[i] <= '9'){
                numSeen = true;
            }else if(str[i] == '.'){
                //.之前不能出现.或者e
                if(dotSeen || eSeen){
                    return false;
                }
                dotSeen = true;
            }else if(str[i] == 'e' || str[i] == 'E'){
                //e之前不能出现e，必须出现数
                if(eSeen || !numSeen){
                    return false;
                }
                eSeen = true;
                numSeen = false;//重置numSeen，排除123e或者123e+的情况,确保e之后也出现数
            }else if(str[i] == '-' || str[i] == '+'){
                //+-出现在0位置或者e/E的后面第一个位置才是合法的
                if(i != 0 && str[i-1] != 'e' && str[i-1] != 'E'){
                    return false;
                }
            }else{//其他不合法字符
                return false;
            }
        }
        return numSeen;
    }
}
```

## 剑指 Offer 21. 调整数组顺序使奇数位于偶数前面

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

>   示例：
>
>   输入：nums = [1,2,3,4]
>
>   输出：[1,3,2,4] 
>
>   注：[3,1,2,4] 也是正确的答案之一。
>
>
>   提示：
>
>   1 <= nums.length <= 50000
>   1 <= nums[i] <= 10000

```java
class Solution {
    public int[] exchange(int[] nums) {
        if(nums.length == 0 || nums == null) {
            return nums;
        }
        int i = 0, j = nums.length-1;
        while(i != j){
            // 找到左边第一个偶数
            while(nums[i] % 2 != 0 && i < j ){
                i++;
            }
            // 找到右边第一个奇数
            while(nums[j] % 2 == 0 && i < j ){
                j--;
            }
            if(i < j){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
            }
        }
        return nums;
    }
}
```

## 剑指 Offer 22. 链表中倒数第k个节点

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

>   示例：
>
>   给定一个链表: 1->2->3->4->5, 和 k = 2.
>
>   返回链表 4->5.
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
    public ListNode getKthFromEnd(ListNode head, int k) {
        if (head == null || k == 0) {
            return null;
        }
         //快慢指针
        ListNode slow = head;
        ListNode fast = head;
        //快指针指向慢指针前k个位置
        for(int i = 0; i < k; i++){
            if(fast == null){
                return null;
            }
            fast = fast.next;
        }
        //当快指针遍历完成后，返回慢指针位置
        while(fast != null){
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```

## 剑指 Offer 24. 反转链表

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

>   示例:
>
>   输入: 1->2->3->4->5->NULL
>
>   输出: 5->4->3->2->1->NULL
>
>
>   限制：
>
>   0 <= 节点个数 <= 5000
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
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode pre = null;
        ListNode next = null;
        //三个指针，pre、head、next
        while(head != null){
            next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
    //递归解法
    public ListNode reverseList2 (ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode last = reverseList2(head.next);
        head.next.next = head;
        head.next = null;
        return last;
    }
}
```

## 剑指 Offer 25. 合并两个排序的链表

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

>   示例1：
>
>   输入：1->2->4, 1->3->4
>
>   输出：1->1->2->3->4->4
>
>   限制：
>
>   0 <= 链表长度 <= 1000
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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        //创建头节点
        ListNode head = new ListNode(0);
        ListNode newList = head;
        while(true){
            //当list1为null时，直接复制list2剩余元素
            if(list1 == null){
                while(list2 != null){
                    newList.next = list2;
                    newList = newList.next;
                    list2 = list2.next;
                }
                return head.next;
            }
            //当list2为null时，直接复制list1剩余元素
            if(list2 == null){
                while(list1 != null){
                    newList.next = list1;
                    newList = newList.next;
                    list1 = list1.next;
                }
                return head.next;
            }
            //先添加小的
            if(list1.val <= list2.val){
                newList.next = list1;
                newList = newList.next;
                list1 = list1.next;
            }else{
                newList.next = list2;
                newList = newList.next;
                list2 = list2.next;
            }
        }
    }
}
```

## 剑指 Offer 26. 树的子结构

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:

给定的树 A:

```
     3
    / \
   4   5
  / \
 1   2
```

给定的树 B：

```
   4 
  /
 1
```

返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

>   示例 1：
>
>   输入：A = [1,2,3], B = [3,1]
>
>   输出：false
>
>   示例 2：
>
>   输入：A = [3,4,5,1,2], B = [4,1]
>
>   输出：true
>
>   限制：
>
>   0 <= 节点个数 <= 10000
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
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        // 如果A或B都走完了还没找到，那应该就是找不到了
        if(A == null || B == null) return false;
        //看看当前节点成不？不成就看看左边，不然看看右边？
        return subTree(A, B) 
            || isSubStructure(A.left, B) 
            || isSubStructure(A.right, B);
    }

    boolean subTree(TreeNode a, TreeNode b){
        // b这边都看完了，还没挑出不同？那就是了吧！
        if(b == null) return true;
        // b这边还没看完了，a那边就null了？
        else if(a == null) return false;
        return a.val == b.val 
            && subTree(a.left, b.left) 
            && subTree(a.right, b.right);
    }
}
```

## 剑指 Offer 27. 二叉树的镜像

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

     	  4
        /   \
      2     7
     / \   / \
    1   3 6   9
镜像输出：

    	 4
        /   \
      7     2
     / \   / \
    9   6 3   1
>   示例 1：
>
>   输入：root = [4,2,7,1,3,6,9]
>
>   输出：[4,7,2,9,6,3,1]
>
>
>   限制：
>
>   0 <= 节点个数 <= 1000
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
    public TreeNode mirrorTree(TreeNode root) {
        if(root == null){
            return null;
        }
        
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        
        mirrorTree(root.left);
        mirrorTree(root.right);

        return root;
    }
}
```

## 剑指 Offer 28. 对称的二叉树

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    	1
       / \
      2   2
     / \ / \
    3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    	1
       / \
      2   2
       \   \
       3    3
>   示例 1：
>
>   输入：root = [1,2,2,3,4,4,3]
>
>   输出：true
>
>   示例 2：
>
>   输入：root = [1,2,2,null,3,null,3]
>
>   输出：false
>
>
>   限制：
>
>   0 <= 节点个数 <= 1000
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
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        return isMirrorBTree(root.left, root.right);
    }
    
    public boolean isMirrorBTree(TreeNode root1, TreeNode root2) {
        if(null == root1 && null == root2) {
            return true;
        } else if(null == root1 || null == root2) {
            return false;
        }
        if(root1.val != root2.val) {
            return false;
        }
        //此处注意相反比较
        return isMirrorBTree(root1.left, root2.right)
            && isMirrorBTree(root1.right, root2.left);
    }
}
```

## 剑指 Offer 29. 顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

>   示例 1：
>
>   输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
>
>   输出：[1,2,3,6,9,8,7,4,5]
>
>   示例 2：
>
>   输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
>
>   输出：[1,2,3,4,8,12,11,10,9,5,6,7]
>
>
>   限制：
>
>   0 <= matrix.length <= 100
>
>   0 <= matrix[i].length <= 100

```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if(matrix.length == 0 || matrix[0].length == 0){
            return new int[0];
        }
        //定义四个边界
        int low = 0;
        int high = matrix.length -1;
        int left = 0;
        int right = matrix[0].length -1;

        int len = (high + 1) * (right + 1);
        int[] result = new int[len];
        int p = 0;
        
        while(low <= high && left <= right){
            for(int i = left; i <= right; i++){
                result[p++] = matrix[low][i];
            }
            for(int i = low+1; i <= high; i++){
                result[p++] = matrix[i][right];
            }
            if(low < high){
               for(int i = right-1; i >= left; i--){
                    result[p++] = matrix[high][i];
                } 
            }
            if(left < right){
                for(int i = high-1; i >= low+1; i--){
                    result[p++] = matrix[i][left];
                }
            }
            low++;
            high--;
            left++;
            right--;
        }
        return result;
    }
}
```

## 剑指 Offer 30. 包含min函数的栈

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

>   示例:
>
>   ```java
>   MinStack minStack = new MinStack();
>   minStack.push(-2);
>   minStack.push(0);
>   minStack.push(-3);
>   minStack.min();   --> 返回 -3.
>   minStack.pop();
>   minStack.top();      --> 返回 0.
>   minStack.min();   --> 返回 -2.
>   ```
>
>
>   提示：
>
>   各函数的调用总次数不超过 20000 次
>

```java
class MinStack {
    
    /** initialize your data structure here. */
    Stack<Integer> stackAll;
    Stack<Integer> stackMin;
    
    public MinStack() {
        stackAll = new Stack<>();
        stackMin = new Stack<>();
    }
    
    public void push(int x) {
        if (stackMin.isEmpty()) {
            stackMin.push(x);
        } else {
            if (x < stackMin.peek()) {
                stackMin.push(x);
            } else {
                stackMin.push(stackMin.peek());
            }
        }
        stackAll.push(x);
    }
    
    public void pop() {
        stackAll.pop();
        stackMin.pop();
    }
    
    public int top() {
        return stackAll.peek();
    }
    
    public int min() {
        return stackMin.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```

## 剑指 Offer 31. 栈的压入、弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

>   示例 1：
>
>   输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
>
>   输出：true
>
>   解释：我们可以按以下顺序执行：
>
>   push(1), push(2), push(3), push(4), pop() -> 4,
>
>   push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
>
>   示例 2：
>
>   输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
>
>   输出：false
>
>   解释：1 不能在 2 之前弹出。
>
>
>   提示：
>
>   0 <= pushed.length == popped.length <= 1000
>
>   0 <= pushed[i], popped[i] < 1000
>
>   pushed 是 popped 的排列。

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack<>();
        int count = 0;
        for(int i = 0; i < pushed.length; i++){
            stack.push(pushed[i]);
            while(!stack.isEmpty() && stack.peek() == popped[count]){
                stack.pop();
                count++;
            }
        }
        return stack.isEmpty();
    }
}
```

## 剑指 Offer 32 - I. 从上到下打印二叉树

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

>   例如:
>
>   给定二叉树: [3,9,20,null,null,15,7],
>
>       	3
>          / \
>         9  20
>           /  \
>          15   7
>
>   返回：
>
>   [3,9,20,15,7]
>
>
>   提示：
>
>   节点总数 <= 1000
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
    public int[] levelOrder(TreeNode root) {
        if (root == null) {
            return new int[0];
        }
        LinkedList<TreeNode> queue = new LinkedList<>();
        List<Integer> res = new ArrayList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            res.add(node.val);
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
        int[] result = new int[res.size()];
        for (int i = 0; i < res.size(); i++) {
            result[i] = res.get(i);
        }
        return result;
    }
}
```

## 剑指 Offer 32 - II. 从上到下打印二叉树 II

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

>   例如:
>
>   给定二叉树: [3,9,20,null,null,15,7],
>
>       	3
>          / \
>         9  20
>           /  \
>          15   7
>   返回其层次遍历结果：
>
>   ```
>   [
>     [3],
>     [9,20],
>     [15,7]
>   ]
>   ```
>
>
>   提示：
>
>   节点总数 <= 1000
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
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root == null) 
            return new LinkedList<>();
        List<List<Integer>> treeList = new LinkedList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int count = 0;
        while(!queue.isEmpty()){
            //记录当前这一层所有的节点数
            count = queue.size();
            List<Integer> tempList = new LinkedList<>();
            while(count > 0){
                TreeNode temp = queue.poll();
                tempList.add(temp.val);
                if(temp.left != null) 
                    queue.offer(temp.left);
                if(temp.right != null) 
                    queue.offer(temp.right);
                count--;
            }
            treeList.add(tempList);
        }  
        return treeList;
    }
}
```

## 剑指 Offer 32 - III. 从上到下打印二叉树 III

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

>   例如:
>
>   给定二叉树: [3,9,20,null,null,15,7],
>
>       	3
>          / \
>         9  20
>           /  \
>          15   7
>   返回其层次遍历结果：
>
>   ```
>   [
>     [3],
>     [20,9],
>     [15,7]
>   ]
>   ```
>
>
>   提示：
>
>   节点总数 <= 1000
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
    public List<List<Integer>> levelOrder(TreeNode root) {
        Deque<TreeNode> d = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null) d.add(root);
        while(!d.isEmpty()){
            LinkedList<Integer> tmp = new LinkedList<>();
            for(int i = d.size();i > 0;i--){
                TreeNode node = d.poll();
                tmp.addLast(node.val);
                if(node.left != null) d.addLast(node.left);
                if(node.right != null) d.add(node.right);
            }
            if(res.size() % 2 == 1) Collections.reverse(tmp);
            res.add(tmp);
        }
        return res;
    }
}
```

## 剑指 Offer 33. 二叉搜索树的后序遍历序列

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

参考以下这颗二叉搜索树：

         5
        / \
       2   6
      / \
     1   3
>
>   示例 1：
>
>   输入: [1,6,3,2,5]
>
>   输出: false
>
>   示例 2：
>
>   输入: [1,3,2,6,5]
>
>   输出: true
>
>
>   提示：
>
>   数组长度 <= 1000
>

```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        int len = postorder.length;
        if(len == 0) return true;
        //根节点
        int root = postorder[len - 1];
        int i;
        //找到第一个大于根节点的下标i;
        for(i = 0; i < len - 1; i++){
            if(postorder[i] > root) break;
        }
        //i右边所有节点都应该大于根节点，若有小于，直接返回false;
        for(int j = i; j < len - 1; j++){
            if(postorder[j] < root){
                return false;
            }
        }
        //递归判断i左边和右边是否满足后序遍历序列
        boolean left = true, right = true;
        if(i > 0){
            left = verifyPostorder(Arrays.copyOfRange(postorder, 0, i));
        }
        if(i < len - 1){
           right = verifyPostorder(Arrays.copyOfRange(postorder, i, len-1)); 
        }
        return left && right;  
    }
}
```

## 剑指 Offer 34. 二叉树中和为某一值的路径

输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径

>   示例:
>
>   给定如下二叉树，以及目标和 sum = 22，
>
>                 5
>                / \
>               4   8
>              /   / \
>             11  13  4
>            /  \    / \
>           7    2  5   1
>   返回:
>
>   ```
>   [
>      [5,4,11,2],
>      [5,8,4,5]
>   ]
>   ```

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
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        if(root == null) 
            return res;
        dfs(root, sum);
        return res;
    }

    private void dfs(TreeNode node, int target){
        if(node == null)
            return;
        list.add(node.val);
        target -= node.val;
        if(target == 0 && node.left == null && node.right == null){
            res.add(new ArrayList<>(list)); //注意浅拷贝
        }
        dfs(node.left,target);
        dfs(node.right,target);
        list.remove(list.size() - 1);
    }
}
```

## 剑指 Offer 35. 复杂链表的复制

请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

>   示例 1：
>
>   ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png)
>
>   输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
>
>   输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
>
>   示例 2：
>
>   ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png)
>
>   输入：head = [[1,1],[2,1]]
>
>   输出：[[1,1],[2,1]]
>
>   示例 3：
>
>   ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png)
>
>   输入：head = [[3,null],[3,0],[3,null]]
>
>   输出：[[3,null],[3,0],[3,null]]
>
>   示例 4：
>
>   输入：head = []
>
>   输出：[]
>
>   解释：给定的链表为空（空指针），因此返回 null。
>
>
>   提示：
>
>   -10000 <= Node.val <= 10000
>
>   Node.random 为空（null）或指向链表中的节点。
>
>   节点数目不超过 1000 。

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;
    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
 public Node copyRandomList(Node head) {
        Map<Node, Node> map = new HashMap();
        if(head == null) return null;
        Node p = head;
        //哈希表存储所有的新旧节点对儿
        while(p != null){
            map.put(p, new Node(p.val));
            p = p.next;
        }
        p = head;
        while(p != null){
            //新节点的next就是旧节点next对应的新节点
            map.get(p).next = map.get(p.next);
            //新节点的random就是旧节点random对应的新节点
            map.get(p).random = map.get(p.random);
            p = p.next;
        }
        return map.get(head);
    }
}
```

## 剑指 Offer 36. 二叉搜索树与双向链表

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

为了让您更好地理解问题，以下面的二叉搜索树为例：

<img src="https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png" alt="img" style="zoom:50%;" />

我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。

<img src="https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png" alt="img" style="zoom:50%;" />

特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node() {}
    public Node(int _val) {
        val = _val;
    }
    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
    Node pre = null;
    public Node treeToDoublyList(Node root) {
        if(root == null) {
            return null;
        }
        Node p = root, q = root;
        // 寻找最左和最右节点
        while(p.left != null) {
            p = p.left;
        }
        while(q.right != null) {
            q = q.right;
        }
        // 中序遍历
        inorder(root);
        // 进行头尾相接
        p.left = q;
        q.right = p;
        return p;
    }
    // 形成的是一个非循环的双向链表
    public void inorder(Node curr){
        if(curr == null) {
            return;
        }
        inorder(curr.left);
        // 1.将当前被访问节点curr的左孩子置为前驱pre（中序）
        curr.left = this.pre;
        // 2.若前驱pre不为空，则前驱的右孩子置为当前被访问节点curr
        if(this.pre != null) {
           this.pre.right = curr; 
        }
        // 3.将前驱pre指向当前节点curr，即访问完毕
        pre = curr;
        inorder(curr.right);
    }
}
```

## 剑指 Offer 37. 序列化二叉树

请实现两个函数，分别用来序列化和反序列化二叉树。

>   示例: 
>
>   你可以将以下二叉树：
>
>       	1
>          / \
>         2   3
>            / \
>           4   5
>   序列化为 "[1,2,3,null,null,4,5]"
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
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null){
            return "";
        }
        return helpSerialize(root, new StringBuilder()).toString();
    }

    StringBuilder helpSerialize(TreeNode root, StringBuilder s) {
        if (root == null) return s;
        s.append(root.val).append("!");
        if (root.left != null) {
            helpSerialize(root.left, s);
        } else {
            s.append("#!"); // 为null的话直接添加即可
        }
        if (root.right != null) {
            helpSerialize(root.right, s);
        } else {
            s.append("#!");
        }
        return s;
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String str) {
        if (str == null || str.length() == 0) return null;
        String[] split = str.split("!");
        return helpDeserialize(split);
    }

    int index = 0;
    TreeNode helpDeserialize(String[] strings) {
        if (strings[index].equals("#")) {
            index++;// 数据前进
            return null;
        }
        // 当前值作为节点已经被用
        TreeNode root = new TreeNode(Integer.valueOf(strings[index]));
        index++; // index++到达下一个需要反序列化的值
        root.left = helpDeserialize(strings);
        root.right = helpDeserialize(strings);
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

## 剑指 Offer 38. 字符串的全排列

输入一个字符串，打印出该字符串中字符的所有排列。

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

>   示例:
>
>   输入：s = "abc"
>
>   输出：["abc","acb","bac","bca","cab","cba"]

```java
class Solution {
    // 使用set去重
    Set<String> res = new HashSet<>();
    public String[] permutation(String s) {
        if(s == null) {
            return new String[]{};
        }
        // 判断每一个字符是使用过
        boolean[] visited = new boolean[s.length()];
        dfs(s, "", visited);    //看作剩下的字符有多少情况
        String[] result = new String[res.size()];
        return res.toArray(result);
    }
    // s: 原字符串
    // letter：当前拥有的字符串
    private void dfs(String s, String letter, boolean[] visited) {
        // 长度一致，退出递归
        if(s.length() == letter.length()){
            res.add(letter);
            return; 
        }
        for(int i = 0; i < s.length(); i++){
            // 跳过已被使用的字符串
            if(visited[i]) {
                continue;
            }
            char c = s.charAt(i);
            visited[i] = true;
            dfs(s, letter + c, visited);
            visited[i] = false;
        }
    }
}
```

## 剑指 Offer 39. 数组中出现次数超过一半的数字

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

>   示例 1:
>
>   输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
>
>   输出: 2

```java
class Solution {
    public int majorityElement(int[] nums) {
        // 初始化为数组的第一个元素，接下来用于记录上一次访问的值
        int target = nums[0];
        // 用于记录出现次数
		int count = 1;
		for(int i = 1; i < nums.length; i++) {
			if(target == nums[i]) {
				count++;
			}else {
				count--;
			}
			if(count == 0) {
                // 当count=0时，更换target的值为当前访问的数组元素的值，次数设为1
				target = nums[i];
				count = 1;
			}
		}
		return target;
    }
}
```

## 剑指 Offer 40. 最小的k个数

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

>   示例 1：
>
>   输入：arr = [3,2,1], k = 2
>
>   输出：[1,2] 或者 [2,1]
>
>   示例 2：
>
>   输入：arr = [0,1,2,1], k = 1
>
>   输出：[0]

**方法一：堆**

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (k == 0) {
            return new int[0];
        }
        // 使用一个最大堆（大顶堆）
        // Java 的 PriorityQueue 默认是小顶堆，添加 comparator 参数使其变成最大堆
        Queue<Integer> heap = new PriorityQueue<>(k, (i1, i2) -> Integer.compare(i2, i1));
        for (int e : arr) {
            // 当前数字小于堆顶元素才会入堆
            if (heap.isEmpty() || heap.size() < k || e < heap.peek()) {
                heap.offer(e);
            }
            if (heap.size() > k) {
                heap.poll(); // 删除堆顶最大元素
            }
        }
        // 将堆中的元素存入数组
        int[] res = new int[heap.size()];
        int j = 0;
        for (int e : heap) {
            res[j++] = e;
        }
        return res;
    }
}
```

方法二：快排选择

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (k == 0) {
            return new int[0];
        } else if (arr.length <= k) {
            return arr;
        }

        // 原地不断划分数组
        partitionArray(arr, 0, arr.length - 1, k);

        // 数组的前 k 个数此时就是最小的 k 个数，将其存入结果
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = arr[i];
        }
        return res;
    }

    void partitionArray(int[] arr, int lo, int hi, int k) {
        // 做一次 partition 操作
        int m = partition(arr, lo, hi);
        // 此时数组前 m 个数，就是最小的 m 个数
        if (k == m) {
            // 正好找到最小的 k(m) 个数
            return;
        } else if (k < m) {
            // 最小的 k 个数一定在前 m 个数中，递归划分
            partitionArray(arr, lo, m-1, k);
        } else {
            // 在右侧数组中寻找最小的 k-m 个数
            partitionArray(arr, m+1, hi, k);
        }
    }

    // partition 函数和快速排序中相同，具体可参考快速排序相关的资料
    int partition(int[] a, int lo, int hi) {
        int i = lo;
        int j = hi + 1;
        int v = a[lo];
        while (true) { 
            while (a[++i] < v) {
                if (i == hi) {
                    break;
                }
            }
            while (a[--j] > v) {
                if (j == lo) {
                    break;
                }
            }

            if (i >= j) {
                break;
            }
            swap(a, i, j);
        }
        swap(a, lo, j);

        // a[lo .. j-1] <= a[j] <= a[j+1 .. hi]
        return j;
    }

    void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}
```

## 剑指 Offer 41. 数据流中的中位数

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

*   void addNum(int num) - 从数据流中添加一个整数到数据结构中。
*   double findMedian() - 返回目前所有元素的中位数。

>   示例 1：
>
>   ```
>   输入：
>   ["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
>   [[],[1],[2],[],[3],[]]
>   输出：
>   [null,null,null,1.50000,null,2.00000]
>   ```
>
>   示例 2：
>
>   ```
>   输入：
>   ["MedianFinder","addNum","findMedian","addNum","findMedian"]
>   [[],[2],[],[3],[]]
>   输出：
>   [null,null,2.00000,null,2.50000]
>   ```

```java
class MedianFinder {
    private PriorityQueue<Integer> lowPart;
    private PriorityQueue<Integer> highPart;
    int size;
    /** initialize your data structure here. */
    public MedianFinder() {
        lowPart = new PriorityQueue<Integer>((x, y) -> y - x);  //最大堆
        highPart = new PriorityQueue<Integer>();
        size = 0;
    }
    
    public void addNum(int num) {
        size++;
        lowPart.offer(num);
        highPart.offer(lowPart.poll());
        if((size & 1) == 1){
            lowPart.offer(highPart.poll());
        }
    }
    
    public double findMedian() {
        if((size & 1) == 1){
            return (double) lowPart.peek();
        }else{
            return (double) (lowPart.peek() + highPart.peek()) / 2;
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

## 剑指 Offer 42. 连续子数组的最大和

输入一个整型数组，数组里有正数也有负数。数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

>   示例1:
>
>   输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
>
>   输出: 6
>
>   解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        // 用sum动态地记录当前节点最大连续子数组和
        int sum = 0;
        // 用max动态记录最大值
        int max = nums[0];
        for(int n : nums){
            // 如果当前和小于0，只会连累后面的数组和，干脆放弃！
            if(sum >= 0) {
                sum += n;
            } else {
                sum = n; 
            }
            // 更新max
            max = Math.max(max, sum);
        }
        return max;
    }
}
```

## 剑指 Offer 43. 1～n整数中1出现的次数

输入一个整数 n ，求1～n这n个整数的十进制表示中1出现的次数。

例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。

>   示例 1：
>
>   输入：n = 12
>
>   输出：5
>
>   示例 2：
>
>   输入：n = 13
>
>   输出：6

```java
class Solution {
        private int dfs(int n) {
        if (n <= 0) {
            return 0;
        }
        String numStr = String.valueOf(n);
        int high = numStr.charAt(0) - '0';
        int pow = (int) Math.pow(10, numStr.length() - 1);
        int last = n - high * pow;
        if (high == 1) {
            // 最高位是1，如1234, 此时pow = 1000,那么结果由以下三部分构成：
            // (1) dfs(pow - 1)代表[0,999]中1的个数;
            // (2) dfs(last)代表234中1出现的个数;
            // (3) last+1代表固定高位1有多少种情况。
            return dfs(pow - 1) + dfs(last) + last + 1;
        } else {
            // 最高位不为1，如2234，那么结果也分成以下三部分构成：
            // (1) pow代表固定高位1，有多少种情况;
            // (2) high * dfs(pow - 1)代表999以内和1999以内低三位1出现的个数;
            // (3) dfs(last)同上。
            return pow + high * dfs(pow - 1) + dfs(last);
        }
    }
    // 递归求解
    public int countDigitOne(int n) {
        return dfs(n);
    }
}
```

## 剑指 Offer 44. 数字序列中某一位的数字

数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。

请写一个函数，求任意第n位对应的数字。

>   示例 1：
>
>   输入：n = 3
>
>   输出：3
>
>   示例 2：
>
>   输入：n = 11
>
>   输出：0

```java
class Solution {
    public int findNthDigit(int n) {
        if (n <= 9) return n;
        double N = (double)n;
        int len = 1;
        double standard = Math.pow(10, len-1) * 9 * len;
        while (N > standard) {
            // System.out.println("N:" + N + "  standard:" + standard+"  len:"+len);
            N = N - standard;
            len++;
            standard = Math.pow(10, len-1) * 9 * len;//这一步注意越界
        }
        //以上步骤判断下标为n的数字，属于哪个自然数
        /*
            1-9 9个数，占用9x1个下标
            10-99 90个数，占用90x2个下标
            100-999 900个数，占用900x3个下标
            1000-9999 90000个数，占用9000x4个下标
            ......
        */
        int Number = (int)N/len;//求出这是从100..000开始的第几个数
        int mod = (int)N%len;//求出是数字中的第几位
        // System.out.println("number:"+Number+" mod:"+mod);
        if (mod == 0) {
            return ((int)Math.pow(10, len-1)+Number-1)%10;
        }
        else {
            int target = (int)Math.pow(10, len-1) + Number;
            String s = String.valueOf(target);
            char res = s.charAt(mod-1);
            return res-'0';
        }
    }
}
```

## 剑指 Offer 45. 把数组排成最小的数

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

>   示例 1:
>
>   输入: [10,2]
>
>   输出: "102"
>
>   示例 2:
>
>   输入: [3,30,34,5,9]
>
>   输出: "3033459"

```java
class Solution {
    public String minNumber(int[] nums) {
        List<String> strList = new ArrayList<>();
        for (int num : nums) {
            strList.add(String.valueOf(num));
        }
        strList.sort((s1, s2) -> (s1 + s2).compareTo(s2 + s1));
        StringBuilder sb = new StringBuilder();
        for (String str : strList) {
            sb.append(str);
        }
        return sb.toString();
    }
}
```

## 剑指 Offer 46. 把数字翻译成字符串

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

>   示例 1:
>
>   输入: 12258
>
>   输出: 5
>
>   解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"

```java
class Solution {
    public int translateNum(int num) {
        if (num <= 9) {
            return 1;
        }
        //xyzcba，先取最后两位（个位和十位）即ba，
        int ba = num % 100;
        //如果 ba>=26 或者 ba<=9，此时只能分解成f(xyzcb);
        if (ba <= 9 || ba >= 26) {
            return translateNum(num / 10);
        }
        //否则能分解成f(xyzcb) + f(xyzc)
        else  {
            return translateNum(num / 10) + translateNum(num / 100);
        }
    }
}
```

## 剑指 Offer 47. 礼物的最大价值

在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

>   示例 1:
>
>   ```
>   输入: 
>   [
>     [1,3,1],
>     [1,5,1],
>     [4,2,1]
>   ]
>   输出: 12
>   解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
>   ```

```java
class Solution {
    public int maxValue(int[][] grid) {
        if(grid == null) {
            return 0;
        }
        // m行n列
        int m = grid.length;
        int n = grid[0].length;
        // 创建一个长度+1的二维数组，从1开始遍历grid
        int[][] f = new int[m+1][n+1];
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                // 
                f[i+1][j+1] = Math.max(f[i][j+1], f[i+1][j]) + grid[i][j];
            }
        }
        return f[m][n];
    }
}
```

## 剑指 Offer 48. 最长不含重复字符的子字符串

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

>   示例 1:
>
>   输入: "abcabcbb"
>
>   输出: 3 
>
>   解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
>
>   示例 2:
>
>   输入: "bbbbb"
>
>   输出: 1
>
>   解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
>
>   示例 3:
>
>   输入: "pwwkew"
>
>   输出: 3
>
>   解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
>
>   请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length()==0) {
            return 0;
        }
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        //记录最大长度
        int max = 0;
        //记录最左开始字母下标
        int left = 0;
        for(int i = 0; i < s.length(); i ++){
            if(map.containsKey(s.charAt(i))){
                //更新最左开始字母下标
                left = Math.max(left,map.get(s.charAt(i)) + 1);
            }
            map.put(s.charAt(i),i);
            max = Math.max(max,i-left+1);
        }
        return max;
    }
}
```

## 剑指 Offer 49. 丑数

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

>   示例:
>
>   输入: n = 10
>
>   输出: 12
>
>   解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
>
>   说明:  
>
>   1 是丑数。
>
>   n 不超过1690。

```java
class Solution {
    public int nthUglyNumber(int index) {
        if (index == 0) {
            return 0;
        }
        int[] res = new int[index];
        res[0] = 1;
        int pos2 = 0;// 2的对列
        int pos3 = 0;// 3的对列
        int pos5 = 0;// 5的对列
        // 一个丑数*2/3/5还是丑数，从1开始
        for (int i = 1; i < index; i++) {
            //取三个数中最小的
            res[i] = Math.min(Math.min(res[pos2] * 2, res[pos3] * 3), res[pos5] * 5);
            //将相应数的指针+1
            if (res[i]==res[pos2] * 2) {
                pos2++;
            }
            if (res[i]==res[pos3] * 3) {
                pos3++;
            }
            if (res[i]==res[pos5] * 5) {
                pos5++;
            }
        }
        return res[index-1];
    }
}
```

## 剑指 Offer 50. 第一个只出现一次的字符

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

>   示例:
>
>   s = "abaccdeff"
>
>   返回 "b"
>
>   s = "" 
>
>   返回 " "

```java
class Solution {
    public char firstUniqChar(String str) {
        if(str == null || str.length() == 0){
            return ' ';
        }
        //存储ASCII码
        int[] count = new int[126];
        for(int i = 0; i < str.length(); i++){
            char c = str.charAt(i);
            count[c]++;
        }
        for(int i = 0; i < str.length(); i++){
            char c = str.charAt(i);
            if(count[c] == 1){
                return c;
            }
        }
        return ' ';
    }
}
```

## 剑指 Offer 51. 数组中的逆序对

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

>   示例 1:
>
>   输入: [7,5,6,4]
>
>   输出: 5

```java
class Solution {
    public int reversePairs(int[] nums) {
        return merge(nums, 0, nums.length - 1);
    }
    int merge(int[] arr, int start, int end) {
        if (start >= end) return 0;
        int mid = start + (end - start) / 2;
        int count = merge(arr, start, mid) + merge(arr, mid + 1, end);
        int[] temp = new int[end - start + 1];
        int i = start, j = mid + 1, k = 0;
        while (i <= mid && j <= end) {
            count += arr[i] <= arr[j] ? j - (mid + 1) : 0;
            temp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
        }
        while (i <= mid) {
            count += j - (mid + 1);
            temp[k++] = arr[i++];
        }
        while (j <= end)
            temp[k++] = arr[j++];
        System.arraycopy(temp, 0, arr, start, end - start + 1);
        return count;
    }
}
```

## 剑指 Offer 52. 两个链表的第一个公共节点

输入两个链表，找出它们的第一个公共节点。

如下面的两个链表：

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png" alt="img" style="zoom: 67%;" />

在节点 c1 开始相交。

>   示例 1：
>
>   <img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png" alt="img" style="zoom:67%;" />
>
>   输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
>
>   输出：Reference of the node with value = 8
>
>   输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
>
>   示例 2：
>
>   <img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_2.png" alt="img" style="zoom:67%;" />
>
>   输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
>
>   输出：Reference of the node with value = 2
>
>   输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
>
>   示例 3：
>
>   <img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png" alt="img" style="zoom:67%;" />
>
>   输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
>
>   输出：null
>
>   输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
>
>   解释：这两个链表不相交，因此返回 null。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lengthA = getLength(headA), lengthB = getLength(headB);
        ListNode a = headA, b = headB;
        if(lengthA > lengthB){
            for(int i = 0; i < lengthA - lengthB; i++)
                a = a.next;
        } else {
            for(int i = 0; i < lengthB - lengthA; i++)
                b = b.next;
        }
        while(a != b){
            a = a.next;
            b = b.next;
        }
        return a;
    }
    // 求链表长度
    public int getLength(ListNode head){
        int length = 0;
        for(ListNode temp = head; temp != null; temp = temp.next, length++);
        return length;
    }
}
```

