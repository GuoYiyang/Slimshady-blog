#### 二维数组中的查找

**题目描述**

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

```java
public class Solution {
    public boolean Find(int target, int [][] array) {
        int row = array.length-1;//行号
        int col = 0;//列号
        //从左下角开始搜索
        while(row >= 0 && col < array[0].length){
            if(array[row][col] < target){
                col++;
            }else if(array[row][col] > target){
                row--;
            }else{
                return true;
            }
        }
        return false;
    }
}
```

#### 替换空格

**题目描述**

请实现一个函数，将一个字符串中的每个空格替换成`“%20”`。例如，当字符串为`We Are Happy`.则经过替换之后的字符串为`We%20Are%20Happy`。

```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        StringBuilder sb = new StringBuilder();  
    	for(int i = 0; i < str.length(); i++){
            char c = str.charAt(i);
            if(c == ' '){
                sb.append("%20");
            }else{
                sb.append(c);
            }
        }
        return sb.toString();
    }
}
```

#### 从尾到头打印链表

**题目描述**

输入一个链表，按链表从尾到头的顺序返回一个ArrayList。

```java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*/
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> list = new ArrayList<>();
        if(listNode == null){
            return list;
        }
        ListNode temp = listNode;
        while(temp != null){
            list.add(0, temp.val);
            temp = temp.next;
        }
        return list;
    }
}
```

#### 重建二叉树

**题目描述**

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
import java.util.Arrays;
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre, int [] in) {
        if(pre.length == 0 || in.length == 0){
            return null;
        }
        //前序遍历的第一个节点为根节点
        TreeNode root = new TreeNode(pre[0]);
        for(int i = 0; i < in.length; i++){
            //找到中序遍历中根节点的位置
            if(pre[0] == in[i]){
                root.left = reConstructBinaryTree
                    (Arrays.copyOfRange(pre, 1, i+1), 
                     Arrays.copyOfRange(in, 0, i));
                root.right = reConstructBinaryTree
                    (Arrays.copyOfRange(pre, i+1, pre.length), 
                     Arrays.copyOfRange(in, i+1, in.length));
                break;
            }
        }
        return root;
    }
}
```

#### 用两个栈实现队列

**题目描述**

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

```java
import java.util.Stack;
public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    
    public void push(int node) {
        stack1.push(node);
    }
    
    public int pop() {
        //当栈2为空时，将栈1中的元素反向压入栈2中
        if(stack2.size() == 0){
            while(stack1.size() != 0){
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }
}
```

#### 旋转数组的最小数字

题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。

NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

```java
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        if(array.length == 0){
            return 0;
        }
        //当下一位小于当前位时，返回下一位
        for(int i = 0; i < array.length; i++){
            if(array[i+1] < array[i]){
                return array[i+1];
            }
        }
        return array[0];
    }
}
```

#### 斐波那契数列

**题目描述**

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0，n<=39）。

```java
public class Solution {
    public int Fibonacci(int n) {
        if(n<=1){
            return n;
        }
        return Fibonacci(n-1) + Fibonacci(n-2);
    }
}
```

#### 跳台阶

**题目描述**

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

```java
public class Solution {
    public int JumpFloor(int target) {
        //1	1种
        //2	2种
        //3	3种
        if(target <=3){
            return target;
        }
        //动态规划
        return JumpFloor(target -1) + JumpFloor(target - 2);
    }
}
```

#### 变态跳台阶

**题目描述**

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

```java
/*
f(n)=f(n-1)+f(n-2)+...+f(1)
f(n-1)=f(n-2)+...f(1)
得:f(n)=2*f(n-1)
*/
public class Solution {
    public int JumpFloorII(int target) {
         return 1<<(target-1);
    }
}
```

#### 矩形覆盖

**题目描述**

我们可以用`2*1`的小矩形横着或者竖着去覆盖更大的矩形。请问用`n`个`2*1`的小矩形无重叠地覆盖一个`2*n`的大矩形，总共有多少种方法？

```java
/*
当n=1时，只能竖着覆盖，f(1)=1;
当n=2时，既可以横着覆盖，也可以竖着覆盖，f(2)=2;
当n=N时，只需要考虑最后一块如何覆盖即可，
*/
public class Solution {
    public int RectCover(int target) {
        if(target <=2){
            return target;
        }else{
            //动态规划
            return RectCover(target-1) + RectCover(target-2);
        }
    }
}
```

#### 二进制中1的个数

**题目描述**

输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

**解析**

如果一个整数不为0，那么这个整数至少有一位是1。如果我们把这个整数减1，那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1(如果最右边的1后面还有0的话)。其余所有位将不会受到影响。

举个例子：一个二进制数1100，从右边数起第三位是处于最右边的一个1。减去1后，第三位变成0，它后面的两位0变成了1，而前面的1保持不变，因此得到的结果是1011.我们发现减1的结果是把最右边的一个1开始的所有位都取反了。这个时候如果我们再把原来的整数和减去1之后的结果做与运算，从原来整数最右边一个1那一位开始所有位都会变成0。如1100&1011=1000.也就是说，把一个整数减去1，再和原整数做与运算，会把该整数最右边一个1变成0。那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。

```java
public class Solution {
    public int NumberOf1(int n) {
        int count = 0;
        while(n != 0){
            count++;
            n = n & (n - 1);
        }
        return count;
    }
}
```

#### 数值的整数次方

**题目描述**

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

保证base和exponent不同时为0。

```java
public class Solution {
    public double Power(double base, int exponent) {
        if(base == 0.0){
            return 0.0;
        }
        //保证e为正数
        int e = exponent > 0 ? exponent : -exponent;
        double result = 1.0d;
        for(int i = 0; i < e; i++){
            result *= base;
        }
        //返回正确的结果
        return exponent > 0 ? result : 1.0/result;
  }
}
```

#### 调整数组顺序使奇数位于偶数前面

**题目描述**

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

```java
import java.util.ArrayList;
public class Solution {
    public void reOrderArray(int [] array) {
        if(array == null || array.length == 0){
            return;
        }
        ArrayList<Integer> list = new ArrayList();
        //先保存奇数
        for(int i = 0; i < array.length; i++){
            if(array[i] % 2 != 0){
                list.add(array[i]);
            }
        }
        //在保存偶数
        for(int i = 0; i < array.length; i++){
            if(array[i] % 2 == 0){
                list.add(array[i]);
            }
        }
        //覆盖原数组
        for(int i = 0; i < array.length; i++){
            array[i] = list.get(i);
        }
    }
}
```

#### 链表中倒数第k个结点

**题目描述**

输入一个链表，输出该链表中倒数第k个结点。

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) {
        if(head == null || k ==0 ){
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

#### 反转链表

**题目描述**

输入一个链表，反转链表后，输出新链表的表头。

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) {
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
}
```

#### 合并两个排序的链表

**题目描述**

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode Merge(ListNode list1, ListNode list2) {
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

#### 树的子结构

**题目描述**

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

**解析**

一棵大树 A，一棵小树 B，若 B 是 A 的**子树**，则：

-   B 和 A 的结点值完全相同，它们俩的左子树、右子树所有结点的值也完全相同 
-   或者 B 的左孩子和 A 的结点值完全相同，它们俩的左子树、右子树所有结点的值也完全相同 
-   或者 B 的右孩子和 A 的结点值完全相同，它们俩的左子树、右子树所有结点的值也完全相同

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public boolean HasSubtree(TreeNode root1, TreeNode root2) {
        if(root1 == null || root2 == null){
            return false;
        }
        return judgeSubTree(root1, root2) ||
               judgeSubTree(root1.left, root2) ||
               judgeSubTree(root1.right, root2);  
    }
    
    private boolean judgeSubTree(TreeNode root1, TreeNode root2) {
        if (root2 == null) {
            return true;
        }
        if (root1 == null) {
            return false;
        }
        if (root1.val != root2.val) {
            return judgeSubTree(root1.left, root2) ||
                   judgeSubTree(root1.right, root2);
        }
        return judgeSubTree(root1.left, root2.left) &&
               judgeSubTree(root1.right, root2.right);
    }
}
```

#### 二叉树的镜像

**题目描述**

操作给定的二叉树，将其变换为源二叉树的镜像。

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public void Mirror(TreeNode root) {
        if(root == null){
            return;
        }
       
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        
        Mirror(root.left);
        Mirror(root.right);
    }
}
```

#### 顺时针打印矩阵

**题目描述**

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 * 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10。

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printMatrix(int [][] matrix) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        //定义四个边界
        int low = 0;
        int high = matrix.length -1;
        int left = 0;
        int right = matrix[0].length -1;
        
        while(low <= high && left <= right){
            for(int i = left; i <= right; i++){
                list.add(matrix[low][i]);
            }
            for(int i = low+1; i <= high; i++){
                list.add(matrix[i][right]);
            }
            if(low < high){
               for(int i = right-1; i >= left; i--){
                    list.add(matrix[high][i]);
                } 
            }
            if(left < right){
                for(int i = high-1; i >= low+1; i--){
                    list.add(matrix[i][left]);
                }
            }
            low++;
            high--;
            left++;
            right--;
        }
        return list;
        
    }
}
```

#### 包含min函数的栈

**题目描述**

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

注意：保证测试中不会当栈为空的时候，对栈调用pop()或者min()或者top()方法。

```java
import java.util.Stack;

public class Solution {
	
    Stack<Integer> stackTotal = new Stack<Integer>();
    Stack<Integer> stackLittle = new Stack<Integer>();
    
    public void push(int node) {
        stackTotal.push(node);
        if(stackLittle.isEmpty()){
            stackLittle.push(node);
        }else{
            //stackLittle栈顶始终保存当前最小的元素
            if(node < stackLittle.peek()){
                stackLittle.push(node);
            }else{
                stackLittle.push(stackLittle.peek());
            }
        }
    }
    
    public void pop() {
        stackTotal.pop();
        stackLittle.pop();
    }
    
    public int top() {
        return stackTotal.peek();
    }
    
    public int min() {
        return stackLittle.peek();
    }
}
```

#### 栈的压入、弹出序列

**题目描述**

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。

例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

```java
import java.util.Stack;
public class Solution {
    public boolean IsPopOrder(int [] pushA, int [] popA) {
        Stack<Integer> stack = new Stack<Integer>();
        int count = 0;
        for(int i = 0; i < pushA.length; i++){
            stack.push(pushA[i]);
            while(!stack.isEmpty() && stack.peek() == popA[count]){
                stack.pop();
                count++;
            }
        }
        return stack.isEmpty();
        
    }
}
```

#### 从上往下打印二叉树

**题目描述**

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

```java
import java.util.ArrayList;
import java.util.LinkedList;
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        ArrayList<Integer> list = new ArrayList();
        if(root == null){
            return list;
        }
        LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        
        while(!queue.isEmpty()){
            TreeNode temp = queue.poll();
            list.add(temp.val);
            if(temp.left != null){
                queue.offer(temp.left);
            }
            if(temp.right != null){
                queue.offer(temp.right);
            }
        }
        return list;
    }
}
```

#### 二叉搜索树的后序遍历序列

**题目描述**

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

```java
import java.util.Arrays;
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        int len = sequence.length;
        if(len==0) return false;
        //根节点
        int root = sequence[len - 1];
        int i;
        //找到第一个大于根节点的下标i;
        for(i = 0; i < len - 1; i++){
            if(sequence[i] > root) break;
        }
        //i右边所有节点都应该大于根节点，若有小于，直接返回false;
        for(int j = i; j < len - 1; j++){
            if(sequence[j] < root){
                return false;
            }
        }
        //递归判断i左边和右边是否满足后序遍历序列
        boolean left = true, right = true;
        if(i > 0){
            left = VerifySquenceOfBST(Arrays.copyOfRange(sequence, 0, i));
        }
        if(i < len-1){
           right = VerifySquenceOfBST(Arrays.copyOfRange(sequence, i, len-1)); 
        }
        return left && right;
    }
}
```

#### 二叉树中和为某一值的路径

**题目描述**

输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

```java
import java.util.ArrayList;
public class Solution {
    ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
    ArrayList<Integer> list = new ArrayList<Integer>();
    
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root, int target) {
        if(root == null) return result;
        list.add(root.val);
        target -= root.val;
        if(target == 0 && root.left == null && root.right == null)
            result.add(new ArrayList<Integer>(list));
        FindPath(root.left, target);
        FindPath(root.right, target);
        list.remove(list.size()-1);
        return result;
    }
}
```

#### 复杂链表的复制

**题目描述**

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

```java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
import java.util.HashMap;
public class Solution {
    public RandomListNode Clone(RandomListNode pHead)
    {
        if (pHead == null) {
            return pHead;
        }
        RandomListNode p1 = pHead;
        RandomListNode p2 = pHead;
        HashMap<RandomListNode, RandomListNode> map = new HashMap<>();
        while (p1 != null) {
            map.put(p1, new RandomListNode(p1.label));
            p1 = p1.next;
        }
 
        while (p2 != null) {
            if (p2.next != null) {
                map.get(p2).next = map.get(p2.next);
            } else {
                map.get(p2).next = null;
            }
            map.get(p2).random = map.get(p2.random);
            p2 = p2.next;
        }
        return map.get(pHead);
    }
}
```

#### 二叉搜索树转换为已排序的双向链表

**题目描述**

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
import java.util.ArrayList;
public class Solution {
    public TreeNode Convert(TreeNode pRootOfTree) {
        if(pRootOfTree == null){
            return null;
        }
        ArrayList<TreeNode> list = new ArrayList();
        inOrder(pRootOfTree, list);
        return change(list);
    }
    //中序遍历，结果保存在list数组中
    public void inOrder(TreeNode pRootOfTree, ArrayList<TreeNode> list){
        if(pRootOfTree.left != null){
            inOrder(pRootOfTree.left, list);
        }
        list.add(pRootOfTree);
        if(pRootOfTree.right != null){
            inOrder(pRootOfTree.right, list);
        }
    }
    //修改指针,返回头节点
    public TreeNode change(ArrayList<TreeNode> list){
        for(int i = 0; i < list.size() - 1; i++){
            list.get(i).right = list.get(i + 1);
            list.get(i + 1).left = list.get(i);
        }
        return list.get(0);
    }
}
```

#### 字符串全排列

**题目描述**

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串`abc`,则打印出由字符`a,b,c`所能排列出来的所有字符串`abc,acb,bac,bca,cab`和`cba`。

**输入描述**

输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<String> Permutation(String str) {
       ArrayList<String> result = new ArrayList<>();
       if(str.length() == 0){
           return result;
       }
       recur(str, "", result);
       return result;
    }
    //第一个是str，即当前剩下可以取的string
    //第二个是cur，即当前所拥有的字符串
    //第三个是result，是符合条件的字符串的合集
    public void recur(String str, String cur, ArrayList<String> result){
        //首先需要检测还有没有能取的string，
        if(str.length() == 0){
            //如果没有，便可以开始确认我们当前所拥有的字符串有没有被放进最后的result中
            if(!result.contains(cur)){
                //如果没有，便将他加进去
                result.add(cur);
            }
        }
        //当还有能取的string时，就可以开始进行循环
        //对于现在所剩下的string其中的每一个字符串，都将他尝试与现有的cur进行组合
        for(int i = 0; i < str.length(); i++){
            //已经把当前位于i的字符取过了，第一个argument需要被更新为除这个的剩下的string
            //运用了substring来取i之前的string和i之后的string进行相加
            recur(str.substring(0,i) + str.substring(i+1,str.length()), 
                  cur + str.charAt(i), result);
        }
    }
}
```

#### 数组中次数超过一半的数字

**题目描述**

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组`{1,2,3,2,2,2,5,4,2}`。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

```java
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        //记录每个数字的出现次数
        int[] count = new int[10];
        for(int i = 0; i < array.length; i++){
            count[array[i]]++;
            if(count[array[i]] > array.length/2){
                return array[i];
            }
        }
        return 0;
    }
}
```

#### 最小的K个字符

**题目描述**

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

```java
public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
	ArrayList<Integer> result = new ArrayList<Integer>();
    if(k<= 0 || k > input.length)
        return result;
    //初次排序，完成k个元素的排序
    for(int i = 1; i< k; i++){
        int j = i-1;
        int unFindElement = input[i];
        while(j >= 0 && input[j] > unFindElement){
            input[j+1] = input[j];
            j--;
        }
        input[j+1] = unFindElement;
    }
    //遍历后面的元素 进行k个元素的更新和替换
    for(int i = k; i < input.length; i++){
        if(input[i] < input[k-1]){
            int newK = input[i];
            int j = k-1;
            while(j >= 0 && input[j] > newK){
                input[j+1] = input[j];
                j--;
            }
            input[j+1] = newK;
        }
    }
    //把前k个元素返回
    for(int i=0; i < k; i++)
        result.add(input[i]);
    return result;
}
```

#### 连续子数组的最大和

**题目描述**

{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和(子向量的长度至少是1)。

```java
public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) {
        int i;
        int max = Integer.MIN_VALUE, maxi = -1;
        for(i = 0; i < array.length; i++){
            if(array[i] >= 0){
                break;
            }
            if(array[i] > max){
                max = array[i];
                maxi = i;
            }
        }
        if(i == array.length){
            //全是负数，返回最大的数
            return array[maxi];
        }else{
            //array[i]是第一个大于等于0的数
            int sum = array[i];
            for(int j = i+1; j < array.length - 1; j++){
                if(sum + array[j] > array[j]){
                    sum += array[j];
                }else{
                    sum = array[j];
                }
            }
            //最后一个数进行特判
            return array[array.length - 1] > 0 ? sum + array[array.length - 1] : sum;
        }
    }
}
```

#### 整数中1出现的次数

**题目描述**

求出1-13的整数中1出现的次数,并算出100-1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。

```java
public class Solution {
    public int NumberOf1Between1AndN_Solution(int n) {
        if(n == 0){
            return 0;
        }
        int count = 0;
        for(int i = n; i > 0; i--){
            int num = i;
            //取num的每一位判断是否为1
            while(num > 0){
                if(num%10 == 1){
                    count++;
                }
                num /= 10;
            }
        }
        return count;
    }
}
```

#### 把数组排成最小的数

**题目描述**

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

```java
public class Solution {
    public String PrintMinNumber(int [] numbers) {
        if(numbers == null || numbers.length == 0){
            return "";
        }
        for(int i = 0; i < numbers.length; i++){
            for(int j = i + 1; j < numbers.length; j++){
                if(compare(numbers[i]+"", numbers[j]+"")){
                    int temp = numbers[j];
                    numbers[j] = numbers[i];
                    numbers[i] = temp;
                }
            }
        }
        String result = "";
        for(int i = 0; i < numbers.length; i++){
            result += numbers[i];
        }
        return result;
    }
    
    //str1+str2 > str2+str1 则返回true
    public static boolean compare(String str1, String str2){
        Long num1 = Long.valueOf(str1 + str2);
        Long num2 = Long.valueOf(str2 + str1);
        return num1 - num2 > 0 ? true : false;
    }
}
```

#### 丑数

**题目描述**

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

```java
public class Solution {
    public int GetUglyNumber_Solution(int index) {
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

#### 第一个只出现一次的字符

**题目描述**

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）。

```java
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        if(str == null || str.length() == 0){
            return -1;
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
                return i;
            }
        }
        return -1;
    }
}
```

#### 数组中的逆序对

**题目描述**

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出`P%1000000007`

```java
public class Solution {
    private int cnt;
    private void MergeSort(int[] array, int start, int end){
        if(start>=end)return;
        int mid = (start+end)/2;
        MergeSort(array, start, mid);
        MergeSort(array, mid+1, end);
        MergeOne(array, start, mid, end);
    }
    private void MergeOne(int[] array, int start, int mid, int end){
        int[] temp = new int[end-start+1];
        int k=0,i=start,j=mid+1;
        while(i<=mid && j<= end){
		   //如果前面的元素小于后面的不能构成逆序对
            if(array[i] <= array[j])
                temp[k++] = array[i++];
            else{
			   //如果前面的元素大于后面的，那么在前面元素之后的元素都能和后面的元素构成逆序对
                temp[k++] = array[j++];
                cnt = (cnt + (mid-i+1))%1000000007;
            }
        }
        while(i<= mid)
            temp[k++] = array[i++];
        while(j<=end)
            temp[k++] = array[j++];
        for(int l=0; l<k; l++){
            array[start+l] = temp[l];
        }
    }
    public int InversePairs(int [] array) {
        MergeSort(array, 0, array.length-1);
        return cnt;
    }
}
```

#### 两个链表的第一个公共结点

**题目描述**

输入两个链表，找出它们的第一个公共结点。（注意因为传入数据是链表，所以错误测试数据的提示是用其他方式显示的，保证传入数据是正确的）

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
import java.util.HashSet;
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        if(pHead1 == null || pHead2 == null){
            return null;
        }
        ListNode p1 = pHead1;
        ListNode p2 = pHead2;
        HashSet<ListNode> set = new HashSet<>();
        while(p1 != null){
            set.add(p1);
            p1 = p1.next;
        }
        while(p2 != null){
            if(set.contains(p2)){
                return p2;
            }
            p2 = p2.next;
        }
        return null;
    }
}
```

#### 数字在排序数组中出现的次数

**题目描述**

统计一个数字在排序数组中出现的次数。

```java
import java.util.Arrays;
public class Solution {
    public int GetNumberOfK(int [] array , int k) {
        //二分查找k的下标index
        int index = Arrays.binarySearch(array, k);
        //没有找到
        if (index < 0){
            return 0;
        }
        int cnt = 1;
        //考虑index左右的情况
        for(int i = index+1; i < array.length && array[i] == k; i++){
            cnt++;
        }
        for(int i = index-1; i >= 0 && array[i] == k; i--){
            cnt++;
        }    
        return cnt;
    }
}
```

#### 二叉树的深度

**题目描述**

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public int TreeDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        int left = TreeDepth(root.left);
        int right = TreeDepth(root.right);
        return left > right ? left + 1 : right + 1;
    }
}
```

#### 平衡二叉树

**题目描述**

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

```java
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
        if(root == null){
            return true;
        }
        //左右子树深度差大于1,返回false;
        if(Math.abs(TreeDepth(root.left) - TreeDepth(root.right)) > 1){
            return false;
        }else{
            //递归判断左右子树
            return IsBalanced_Solution(root.left) && IsBalanced_Solution(root.right);
        }
    }
    //求二叉树深度
    public int TreeDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        int left = TreeDepth(root.left);
        int right = TreeDepth(root.right);
        return left > right ? left + 1 : right + 1;
    }
}
```

#### 数组中只出现一次的数字

**题目描述**

一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

```java
//num1,num2分别为长度为1的数组。传出参数
//将num1[0],num2[0]设置为返回结果
import java.util.LinkedList;
import java.util.Arrays;
public class Solution {
    public void FindNumsAppearOnce(int [] array, int num1[] , int num2[]) {
        if(array.length == 0){
           return;
        }
        //先进行排序
        Arrays.sort(array);
        //利用栈的思想
        LinkedList<Integer> list = new LinkedList<>();
        for(int i = 0; i < array.length; i++){
            if(list.isEmpty() || list.peek() != array[i]){
                list.push(array[i]);
            }else if(array[i] == list.peek()){
                list.pop();
            }
        }
        num1[0] = list.poll();
        num2[0] = list.poll();
    }
}
```

#### 和为S的连续正数序列

**题目描述**

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? 

**输出描述**

输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序。

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer> > result = new ArrayList<ArrayList<Integer>>();
        for(int n = (int)(Math.sqrt(8*sum+1) - 1)/2; n > 1; n--){
            if((2*sum - n*n -n) % (2*n) == 0){
                ArrayList<Integer> list = new ArrayList<Integer>();
                int a = (2*sum - n*n -n)/(2*n) + 1;
                for(int i = a; i < a + n; i++){
                    list.add(i);
                }
                result.add(list);
            } 
        }
        return result;
    }
}
```

#### 和为S的两个数字

**题目描述**

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

**输出描述**

对应每个测试案例，输出两个数，小的先输出。

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array, int sum) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        if(array.length == 0){
            return list;
        }
        int slow = 0, high = array.length - 1;
        while(slow < high){
            if(array[slow] + array[high] == sum){
                list.add(array[slow]);
                list.add(array[high]);
                break;
            }else if(array[slow] + array[high] < sum){
                slow++;
            }else{
                high--;
            }
        }
        return list;
    }
}
```

#### 左旋转字符串

**题目描述**

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

```java
public class Solution {
    public String LeftRotateString(String str,int n) {
        if(str.length() == 0 || n == 0){
            return str;
        }
        int len = str.length();
        int toLeft = n % len;
        return str.substring(toLeft) + str.substring(0, toLeft);
    }
}
```

#### 翻转单词顺序列

**题目描述**

牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

```java
import java.util.LinkedList;
public class Solution {
    public String ReverseSentence(String str) {
        if(str.length() == 0 || str == null){
            return str;
        }
        LinkedList<String> list = new LinkedList<String>();
        String[] tmp = str.split(" ");
        //只有一个单词
        if(tmp.length == 0){
            return str;
        }
        //利用栈进行翻转
        for(int i = 0; i < tmp.length - 1; i++){
            list.push(tmp[i]);
            list.push(" ");
        }
        //最后一个单词后没有空格，单独处理
        list.push(tmp[tmp.length - 1]);
        StringBuilder sb = new StringBuilder();
        while(!list.isEmpty()){
            sb.append(list.pop());
        }
        return sb.toString();
    }
}
```

#### 扑克牌顺子

**题目描述**

LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大小王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。

```java
import java.util.TreeSet;
public class Solution {
    public boolean isContinuous(int [] n) {
        if (n.length < 5 || n.length > 5) {
            return false;
        }
        int num = 0;
        //用一个set来填充数据，0不要放进去
        TreeSet<Integer> set = new TreeSet<> ();
        for (int i = 0; i < n.length; i++) {
            if (n[i] == 0) {
                num ++;
            } else {
                set.add(n[i]);
            }
        }
        //set的大小加上0的个数必须为5个。此外set中数值差值在5以内。
        if ((num + set.size()) != 5) {
            return false;
        }
        if ((set.last() - set.first()) < 5) {
            return true;
        }
        return false;
    }
}
```

#### 圆圈中最后剩下的数（约瑟夫环）

**题目描述**

让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)

如果没有小朋友，请返回-1。

```java
import java.util.ArrayList;
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        if(n < 1 || m < 1){
            return -1;
        }
        ArrayList<Integer> list = new ArrayList<>();
        for(int i = 0; i < n; i++){
            list.add(i);
        }
        int cur = -1;
        while(list.size() > 1){
            for(int j = 0; j < m; j++){
                cur++;
                //循环列表
                if(cur == list.size()){
                    cur = 0;
                }
            }
            //移除m-1号
            list.remove(cur);
            //新的list中cur指向了下一个元素，为了保证移动m个准确性，所以cur向前移动一位
            cur--;
        }
        return list.get(0);
    }
}
```

#### 求1+2+3+...+n

**题目描述**

求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

```java
public class Solution {
    public int Sum_Solution(int n) {
        int sum = n;
        //若sum<=0则后面表达式不会执行
        boolean flag = (sum>0) && ((sum+=Sum_Solution(n-1)) > 0);
        return sum;
    }
}
```

#### 不用加减乘除做加法

**题目描述**

写一个函数，求两个整数之和，要求在函数体内不得使用`+、-、*、/`四则运算符号。

```java
public class Solution {
    public int Add(int num1, int num2) {
            while (num2 != 0) {
                int temp = num1^num2;			//相加不考虑进位，异或运算
                num2 = (num1 & num2)<<1;		//计算进位，与运算
                num1 = temp;
            }
            return num1;
    }
}
```

#### 把字符串转换成整数

**题目描述**

将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0

```java
public class Solution {
    public int StrToInt(String str) {
        //最优解
        if(str == null || "".equals(str.trim()))return 0;
        str = str.trim();
        char[] arr = str.toCharArray();
        int i = 0;
        //标记正负值
        int flag = 1;
        int res = 0;
        if(arr[i] == '-'){
            flag = -1;
        }
        if( arr[i] == '+' || arr[i] == '-'){
            i++;
        }
        while(i < arr.length ){
            //是数字
            if(isNum(arr[i])){
                int cur = arr[i] - '0';
                //判断正数非法
                if(flag == 1 && 
                   (res>Integer.MAX_VALUE/10 || res==Integer.MAX_VALUE/10 && cur >7)){
                    return 0;
                }
                //判断负数非法
                if(flag == -1 && 
                   (res>Integer.MAX_VALUE/10 || res==Integer.MAX_VALUE/10 && cur >8)){
                    return 0;
                }
                res = res * 10 + cur;
                i++;
            }else{
                //不是数字
                return 0;
            }
        }
        return res * flag;
    }
    public static boolean isNum(char c){
        return c >= '0' && c <= '9';
    }
}
```

#### 数组中重复的数字

**题目描述**

在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

```java
public class Solution {
    public boolean duplicate(int numbers[], int length, int [] duplication) {
        int[] count = new int[10];
        for(int i = 0; i < length; i++){
            if(count[numbers[i]] == 0){
                count[numbers[i]]++;
            }else{
                duplication[0] = numbers[i];
                return true;
            }
        }
        return false;
    }
}
```

#### 构建乘积数组

**题目描述**

给定一个数组`A[0,1,...,n-1]`,请构建一个数组`B[0,1,...,n-1]`,其中B中的元素B`[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]`。不能使用除法。（注意：规定`B[0] = A[1] * A[2] * ... * A[n-1]，B[n-1] = A[0] * A[1] * ... * A[n-2]`）

```java
import java.util.ArrayList;
public class Solution {
    public int[] multiply(int[] A) {
        int n = A.length;
        int[] B = new int[n];
        B[0] = 1;
        B[n - 1] = 1;
        for(int i = 1; i <= n - 1; i++){
            B[0] *= A[i];
            B[n - 1] *= A[i - 1];
        }
        for(int i = 1; i < n - 1; i++){
            B[i] = 1;
            for(int j = 0; j < n; j++){
                if(j == i) continue;
                B[i] *= A[j];
            }
        }
        return B;
    }
}
```

#### 正则表达式匹配

**题目描述**

请实现一个函数用来匹配包括`'.'`和`'*'`的正则表达式。模式中的字符``'.'``表示任意一个字符，而``'*'``表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但是与`"aa.a"`和`"ab*a"`均不匹配

```java
public class Solution {
    public boolean matchStr(char[] str, int i, char[] pattern, int j) {

        // 边界
        if (i == str.length && j == pattern.length) { // 字符串和模式串都为空
            return true;
        } else if (j == pattern.length) { // 模式串为空
            return false;
        }

        boolean flag = false;
        // 模式串下一个字符是'*'
        boolean next = (j + 1 < pattern.length && pattern[j + 1] == '*');
        if (next) {
            // 要保证i<str.length，否则越界
            if (i < str.length && (pattern[j] == '.' || str[i] == pattern[j])) { 
                return matchStr(str, i, pattern, j + 2) 
                    || matchStr(str, i + 1, pattern, j);
            } else {
                return matchStr(str, i, pattern, j + 2);
            }
        } else {
            if (i < str.length && (pattern[j] == '.' || str[i] == pattern[j])) {
                return matchStr(str, i + 1, pattern, j + 1);
            } else {
                return false;
            }
        }
    }

    public boolean match(char[] str, char[] pattern) {
        return matchStr(str, 0, pattern, 0);
    }
}
```

#### 表示数值的字符串

**题目描述**

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

```java
public class Solution {
    public boolean isNumeric(char[] str) {
        // 标志小数点和指数
        boolean point = false, exp = false;

        for (int i = 0; i < str.length; i++) {
            if (str[i] == '+' || str[i] == '-') {
                // +-号后面必定为数字 或 后面为.（-.123 = -0.123）
                //如果没有下一位或者下一位部位数字或者下一位为小数点，直接返回
                if (i + 1 == str.length 
                    || !(str[i + 1] >= '0' && str[i + 1] <= '9' 
                         || str[i + 1] == '.')) { 
                    return false;
                }
                // +-号只出现在第一位或eE的后一位
                if (!(i == 0 || str[i-1] == 'e' || str[i-1] == 'E')) {
                    return false;
                }
            } else if (str[i] == '.') {
                // .后面必定为数字 或为最后一位（233. = 233.0）
                if (point || exp 
                    || !(i+1 < str.length && str[i+1] >= '0' && str[i+1] <= '9')) { 
                    return false;
                }
                point = true;
            } else if (str[i] == 'e' || str[i] == 'E') {
                // eE后面必定为数字或+-号
                if (exp || i + 1 == str.length 
                        || !(str[i + 1] >= '0' && str[i + 1] <= '9' 
                        || str[i + 1] == '+' 
                        || str[i + 1] == '-')) { 
                    return false;
                }
                exp = true;
            } else if (str[i] >= '0' && str[i] <= '9') {
                continue;
            } else {
                return false;
            }
        }
        return true;
    }
}
```

#### 字符流中第一个不重复的字符

**题目描述**

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符`"go"`时，第一个只出现一次的字符是`"g"`。当从该字符流中读出前六个字符`“google"`时，第一个只出现一次的字符是`"l"`。

**输出描述**

如果当前字符流没有存在出现一次的字符，返回`#`字符。

```java
import java.util.LinkedHashMap;
import java.util.Iterator;
public class Solution {
    LinkedHashMap<Character,Integer> map = new LinkedHashMap<>();
    //Insert one char from stringstream
    public void Insert(char ch)
    {
        if(map.containsKey(ch)) {
            map.put(ch,-1);
        } else {
            map.put(ch, 1);
        }
    }
    //return the first appearence once char in current stringstream
    public char FirstAppearingOnce()
    {
        Iterator<Character> iterator = map.keySet().iterator();
        while (iterator.hasNext()) {
            char cur = iterator.next();
            if(map.get(cur) == 1) {
                return cur;
            }
        }
        return '#';
    }
}
```

#### 链表中环的入口结点

**题目描述**

给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

```java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {

    public ListNode EntryNodeOfLoop(ListNode pHead)
    {    
        if(pHead == null || pHead.next == null){
            return null;
        }
        ListNode slow = pHead;
        ListNode fast = pHead;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow){
                ListNode result = pHead;
                while(result != slow){
                    slow = slow.next;
                    result = result.next;
                }
                return result;
            }
        }
        return null;
    }
}
```

#### 删除链表中重复的结点

**题目描述**

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

```java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public ListNode deleteDuplication(ListNode pHead)
    {
        if(pHead == null || pHead.next == null){
            return pHead;
        }
        //构建头指针
        ListNode head  = new ListNode(0);
        head.next = pHead;
        ListNode pre = head;
        ListNode cur = head.next;
        while(cur != null){
            if(cur.next != null && cur.next.val == cur.val){
                // 相同结点一直前进
                while(cur.next != null && cur.next.val == cur.val){
                    cur = cur.next;
                }
                // 退出循环时，cur 指向重复值，也需要删除，
                // 而 cur.next 指向第一个不重复的值或者null，所以cur 继续前进
                cur = cur.next;
                // pre 连接新结点
                pre.next = cur;
            }else{
                //不相同，pre和cur均前进一位
                pre = cur;
                cur = cur.next;
            }
        }
        return head.next;
    }
}
```

#### 二叉树的下一个结点

**题目描述**

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

```java
/*
public class TreeLinkNode {
    int val;
    TreeLinkNode left = null;
    TreeLinkNode right = null;
    TreeLinkNode next = null;

    TreeLinkNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public TreeLinkNode GetNext(TreeLinkNode pNode)
    {
        // 1.有右子树，下一结点是右子树中的最左结点
        if (pNode.right != null) {
            TreeLinkNode pRight = pNode.right;
            while (pRight.left != null) {
                pRight = pRight.left;
            }
            return pRight;
        }
        // 2.无右子树，且结点是该结点父结点的左子树，则下一结点是该结点的父结点
        if (pNode.next != null && pNode.next.left == pNode) {
            return pNode.next;
        }
        // 3.无右子树，且结点是该结点父结点的右子树，
        //   则我们一直沿着父结点追朔，直到找到某个结点是其父结点的左子树
        if (pNode.next != null) {
            TreeLinkNode pNext = pNode.next;
            while (pNext.next != null && pNext.next.right == pNext) {
                pNext = pNext.next;
            }
            return pNext.next;
        }
        return null;
    }
}
```

#### 对称的二叉树

**题目描述**

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

```java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    boolean isSymmetrical(TreeNode pRoot)
    {
        if(pRoot == null){
            return true;
        }
        return isMirrorBTree(pRoot.left, pRoot.right);
    }
    boolean isMirrorBTree(TreeNode root1, TreeNode root2) {
        if(null == root1 && null == root2) {
            return true;
        }else if(null == root1 || null == root2) {
            return false;
        }
        if(root1.val != root2.val) {
            return false;
        }
        //此处注意相反比较
        return isMirrorBTree(root1.left,root2.right)
            && isMirrorBTree(root1.right,root2.left);
}
}
```

#### 按之字形顺序打印二叉树

**题目描述**

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

```java
import java.util.ArrayList;
import java.util.LinkedList;
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer> > result = new ArrayList<ArrayList<Integer> >();
        if(pRoot == null){
            return result;
        }
        LinkedList<TreeNode> q = new LinkedList<>();  
        q.add(pRoot); 
        boolean rev = true;
        while (!q.isEmpty()) {  
            int size = q.size();
            ArrayList<Integer> list = new ArrayList<>();
            for(int i = 0; i < size; i++){
                TreeNode node = q.poll();
                if(node == null){
                    continue;
                }
                if(rev){
                    //从队列后添加
                    list.add(node.val);
                }else{
                    //从队列前添加
                    list.add(0, node.val);
                }
                //广度优先遍历
                q.add(node.left);
                q.add(node.right);
            }
            if(list.size()!=0){
                result.add(list);
            }
            //取反
            rev = !rev;
        } 
        return result;
    }
}
```

#### 把二叉树打印成多行

**题目描述**

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

```java
import java.util.ArrayList;
import java.util.LinkedList;
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer> > result = new ArrayList<ArrayList<Integer> >();
        if (pRoot == null) {  
            return result;  
        }  
        LinkedList<TreeNode> queue = new LinkedList<>();  
        queue.offer(pRoot);  
        while (!queue.isEmpty()) {  
            ArrayList<Integer> list = new ArrayList<Integer>();
            int size = queue.size();
            for(int i = 0; i < size; i++){
                TreeNode node = queue.poll();
                if(node == null){
                    continue;
                }
                list.add(node.val);
                queue.offer(node.left);
                queue.offer(node.right); 
            }
            if(list.size()!=0){
                result.add(list);
            }
        }  
        return result;
    }
}
```

#### 序列化二叉树

**题目描述**

请实现两个函数，分别用来序列化和反序列化二叉树

二叉树的序列化是指：把一棵二叉树按照某种遍历方式的结果以某种格式保存为字符串，从而使得内存中建立起来的二叉树可以持久保存。序列化可以基于先序、中序、后序、层序的二叉树遍历方式来进行修改，序列化的结果是一个字符串，序列化时通过某种符号表示空节点（#），以！表示一个结点值的结束（value!）。

二叉树的反序列化是指：根据某种遍历顺序得到的序列化字符串结果str，重构二叉树。

```java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    String Serialize(TreeNode root) {
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
    TreeNode Deserialize(String str) {
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
```

#### 二叉搜索树的第k个结点

**题目描述**

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）  中，按结点数值大小顺序第三小结点的值为4。

```java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
import java.util.ArrayList;
public class Solution {
    ArrayList<TreeNode> list = new ArrayList<>();
    
    TreeNode KthNode(TreeNode pRoot, int k)
    {
        if(pRoot == null || k == 0){
            return null;
        }
        inOrder(pRoot);
        if(k > list.size()){
            return null;
        }
        return list.get(k - 1);
    }
    
    public void inOrder(TreeNode root){
        if(root.left != null){
            inOrder(root.left);
        }
        list.add(root);
        if(root.right != null){
            inOrder(root.right);
        }
    }
}
```

#### 数据流中的中位数

**题目描述**

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。

```java
import java.util.Comparator;
import java.util.PriorityQueue;
public class Solution {
    private int cnt = 0;
    private PriorityQueue<Integer> low = new PriorityQueue<>();
    // 默认维护小顶堆
    private PriorityQueue<Integer> high = new PriorityQueue<>(new Comparator<Integer>() 	{
        @Override
        public int compare(Integer o1, Integer o2) {
            return o2.compareTo(o1);
        }
    });

    public void Insert(Integer num) {
        // 数量++
        cnt++;
        // 如果为奇数的话
        if ((cnt & 1) == 1) {
            // 由于奇数，需要存放在大顶堆上
            // 但是呢，现在你不知道num与小顶堆的情况
            // 小顶堆存放的是后半段大的数
            // 如果当前值比小顶堆上的那个数更大
            if (!low.isEmpty() && num > low.peek()) {
                // 存进去
                low.offer(num);
                // 然后在将那个最小的吐出来
                num = low.poll();
            } 
            // 最小的就放到大顶堆，因为它存放前半段
            high.offer(num);
        } else {
            // 偶数的话，此时需要存放的是小的数
            // 注意无论是大顶堆还是小顶堆，吐出数的前提是得有数
            if (!high.isEmpty() && num < high.peek()) {
                high.offer(num);
                num = high.poll();
            } // 大数被吐出，小顶堆插入
            low.offer(num);
        }
    }

    public Double GetMedian() {// 表明是偶数
        double res = 0;
        // 奇数
        if ((cnt & 1) == 1) {
            res = high.peek();
        } else {
            res = (high.peek() + low.peek()) / 2.0;
        }
        return res;
    }
}
```

#### 滑动窗口的最大值

**题目描述**

给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> maxInWindows(int [] num, int size)
    {
        ArrayList<Integer> result = new ArrayList<Integer>();
        if(size > num.length || size <= 0){
            return result;
        }
        int i = 0, j = size - 1;
        while(j < num.length){
            int max = Integer.MIN_VALUE;
            for(int k = i; k <= j; k++){
                if(num[k] > max){
                    max = num[k];
                }
            }
            result.add(max);
            i++;
            j++;
        }
        return result;
    }
}
```

#### 矩阵中的路径

**题目描述**

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。

```java
public class Solution {
    boolean[] visited = null;
    public boolean hasPath(char[] matrix, int rows, int cols, char[] str) {
        visited = new boolean[matrix.length];
        for(int i = 0; i < rows; i++)
            for(int j = 0; j < cols; j++)
                if(subHasPath(matrix,rows,cols,str,i,j,0))
                    return true;
        return false;
    }

    public boolean subHasPath(char[] matrix, int rows, int cols, char[] str, int row, int col, int len){
        if(matrix[row*cols+col] != str[len]|| visited[row*cols+col] == true) 
            return false;
        if(len == str.length-1) return true;
        visited[row*cols+col] = true;
        if(row > 0 && subHasPath(matrix,rows,cols,str,row-1,col,len+1)) 
            return true;
        if(row < rows-1 && subHasPath(matrix,rows,cols,str,row+1,col,len+1)) 
            return true;
        if(col > 0 && subHasPath(matrix,rows,cols,str,row,col-1,len+1)) 
            return true;
        if(col < cols-1 && subHasPath(matrix,rows,cols,str,row,col+1,len+1)) 
            return true;
        visited[row*cols+col] = false;
        return false;
    }
}
```

#### 机器人的运动范围

**题目描述**

地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

```java
import java.util.LinkedList;
public class Solution {
    private final int dx[] = {-1, 1, 0 ,0};
    private final int dy[] = {0, 0, -1, 1};
    public int movingCount(int threshold, int rows, int cols)
    {
        if (threshold <= 0 || rows <= 0 || cols <= 0) {
            return 0;
        }
        int ans = 0;
        boolean[][] vis = new boolean[rows + 1][cols + 1];
        LinkedList<Path> queue = new LinkedList<>();
        queue.add(new Path(0, 0));
        vis[0][0] = true;
        while (!queue.isEmpty()) {
            Path p = queue.poll();
            int x = p.getX();
            int y = p.getY();
            for (int i = 0; i < 4; i++) {
                int xx = x + dx[i];
                int yy = y + dy[i];
                if (xx >= 0 && xx < rows && yy >= 0 && yy < cols && !vis[xx][yy] 
                    && (sum(xx) + sum(yy) <= threshold)) {
                    ans++;
                    vis[xx][yy] = true;
                    queue.add(new Path(xx, yy));
                }
            }
        }
        return ans + 1;
         
    }
    public int sum(int x) {
        int ans = 0;
        while (x != 0) {
            ans += (x % 10);
            x /= 10;
        }
        return ans;
    }
}
 
class Path{
    private int x;
    private int y;
    public Path(int x, int y){
        this.x = x;
        this.y = y;
    }
    public int getX() {
        return x;
    }
    public void setX(int x) {
        this.x = x;
    }
    public int getY() {
        return y;
    }
    public void setY(int y) {
        this.y = y;
    }
}
```

#### 剪绳子

**题目描述**

给你一根长度为n的绳子，请把绳子剪成整数长的m段（m、n都是整数，n>1并且m>1），每段绳子的长度记为k[0],k[1],...,k[m]。请问k[0]xk[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

```java
public class Solution {
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
    *
    * 所以不难发现3和2的个数规律
    */
    public int cutRope(int target) {
        if(target<=1){
            return 0;
        }
        if(target==2){
            return 1;
        }
        if(target==3){
            return 2;
        }
        //数字长度
        int length = target%3==0 ? target/3 : target/3+1;
        //数字后面2的个数
        int length2 = 3 - target % 3;
        int result = 1;
        //算乘积
        for(int i=0; i<length; i++){
            result = result * (i < length - length2 ? 3 : 2);
        }
        return result;
    }
}
```