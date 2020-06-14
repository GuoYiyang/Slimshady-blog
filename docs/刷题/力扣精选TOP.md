## 1. 两数之和

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。 

>   示例:
>
>   给定 nums = [2, 7, 11, 15], target = 9
>
>   因为 nums[0] + nums[1] = 2 + 7 = 9
>
>   所以返回 [0, 1]

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length == 0)  {
            return new int[0];
        }
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++) {
            //key: target - nums[i], value: index
            Integer index = map.get(nums[i]);
            if(index != null) {
                return new int[]{index, i};
            } else {
                map.put(target - nums[i], i);
            }
        }
        return new int[0];
    }
}
```

## 2. 两数相加

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

>   示例：
>
>   输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
>
>   输出：7 -> 0 -> 8
>
>   原因：342 + 465 = 807

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        //预先指针
        ListNode pre = new ListNode(0);
        //头指针
        ListNode cur = pre;
        //记录进位
        int carry = 0;

        while(l1 != null || l2 != null) {

            int x = (l1 == null ? 0 : l1.val);
            int y = (l2 == null ? 0 : l2.val);

            //更新sum和carry
            int sum = x + y + carry;
            carry = sum / 10;
            sum = sum % 10;

            //记录指针节点
            cur.next = new ListNode(sum);
            cur = cur.next;

            if(l1 != null)
                l1 = l1.next;
            if(l2 != null)
                l2 = l2.next;
        }

        //链表全部遍历完毕，carry不为0，继续新增节点
        if(carry != 0) {
            cur.next = new ListNode(carry);
        }

        //返回头指针
        return pre.next;
    }
}
```

## 3. 无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

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
                left = Math.max(left, map.get(s.charAt(i)) + 1);
            }
            map.put(s.charAt(i), i);
            max = Math.max(max, i - left + 1);
        }
        return max;
    }
}
```

## 4. 寻找两个正序数组的中位数

给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。

请你找出这两个正序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。 

>   示例 1:
>
>   nums1 = [1, 3]
>
>   nums2 = [2]
>
>   则中位数是 2.0
>
>   示例 2:
>
>   nums1 = [1, 2]
>
>   nums2 = [3, 4]
>
>   则中位数是 (2 + 3) / 2 = 2.5
>

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        //分配新数组
        int[] nums = new int[m + n];
        //nums1为空
        if (m == 0) {
            //判断奇偶性，分类求中位数
            if (n % 2 == 0) {
                return (nums2[n / 2 - 1] + nums2[n / 2]) / 2.0;
            } else {
                return nums2[n / 2];
            }
        }
        //nums2为空
        if (n == 0) {
            //判断奇偶性，分类求中位数
            if (m % 2 == 0) {
                return (nums1[m / 2 - 1] + nums1[m / 2]) / 2.0;
            } else {
                return nums1[m / 2];
            }
        }
        //新数组指针
        int count = 0;
        //nums1和nums2的指针
        int i = 0, j = 0;
        //遍历复制
        while (count != (m + n)) {
            //nums1数组遍历完，直接复制nums2数组
            if (i == m) {
                while (j != n) {
                    nums[count++] = nums2[j++];
                }
                break;
            }
            //nums2数组遍历完，直接复制nums1数组
            if (j == n) {
                while (i != m) {
                    nums[count++] = nums1[i++];
                }
                break;
            }
            //选择nums1中和nums2中小的加入
            if (nums1[i] < nums2[j]) {
                nums[count++] = nums1[i++];
            } else {
                nums[count++] = nums2[j++];
            }
        }
        //返回中位数
        if (count % 2 == 0) {
            return (nums[count / 2 - 1] + nums[count / 2]) / 2.0;
        } else {
            return nums[count / 2];
        }
    }
}
```

## 5. 最长回文子串

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

>   示例 1：
>
>   输入: "babad"
>
>   输出: "bab"
>
>   注意: "aba" 也是一个有效答案。
>
>   示例 2：
>
>   输入: "cbbd"
>
>   输出: "bb"

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s == null || s.equals("")){
            return s;
        }
        
        //字符串s[i⋯j]是否为回文子串，如果是，dp[i][j]=true，如果不是，dp[i][j]=false
        boolean[][] dp = new boolean[s.length()][s.length()];
        
        //记录最长回文字串的左右下标
        int min = 0, max = 0;
        
        //初始化，单个字符都是回文子串
        for(int i = 0; i < s.length(); i++) {
            dp[i][i] = true;
        }
        
        //开始遍历
        for(int i = s.length() - 1; i >= 0; i--){
            for(int j = i + 1; j < s.length(); j++){
                if(s.charAt(i) == s.charAt(j)) {
                    //考虑“cbba”这种情况
                    if(j - i == 1){
                        dp[i][j] = true;
                    }else{
                    	//只要dp[i+1][j-1]是回文子串，那么dp[i][j]也就是回文子串
                        dp[i][j] = dp[i+1][j-1]; 
                    }
                }else{
                    dp[i][j] = false;
                }

                if(dp[i][j]){
                    //找到更长字串，则更新
                    if(max - min <= j - i){
                        min = i;
                        max = j;
                    }
                }
            }
        }
        return s.substring(min, max + 1);
    }
}
```

## 7. 整数反转

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

>   示例 1:
>
>   输入: 123
>
>   输出: 321
>
>   示例 2:
>
>   输入: -123
>
>   输出: -321
>
>   示例 3:
>
>   输入: 120
>
>   输出: 21
>
>   注意:
>
>   假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。
>
>   请根据这个假设，如果反转后整数溢出那么就返回 0。

```java
class Solution {
      public int reverse(int x) {
        int res = 0;
        while (x != 0) {
            int t = x % 10;
            int newRes = res * 10 + t;
            //如果数字溢出，直接返回0
            if ((newRes - t) / 10 != res)
                return 0;
            res = newRes;
            x = x / 10;
        }
        return res;
    }
}
```

## 8. 字符串转换整数 (atoi)

请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：

*   如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
*   假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
*   该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0 。

提示：

*   本题中的空白字符只包括空格字符 ' ' 。
*   假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−2^31^,  2^31^ − 1]。如果数值超过这个范围，请返回  INT_MAX (2^31^ − 1) 或 INT_MIN (−2^31^) 。

>
>   示例 1:
>
>   输入: "42"
>
>   输出: 42
>
>   示例 2:
>
>   输入: "   -42"
>
>   输出: -42
>
>   解释: 第一个非空白字符为 '-', 它是一个负号。
>
>   我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
>
>   示例 3:
>
>   输入: "4193 with words"
>
>   输出: 4193
>
>   解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
>
>   示例 4:
>
>   输入: "words and 987"
>
>   输出: 0
>
>   解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
>
>   因此无法执行有效的转换。
>
>   示例 5:
>
>   输入: "-91283472332"
>
>   输出: -2147483648
>
>   解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
>
>   因此返回 INT_MIN (−2^31^) 。

```java
class Solution {
    public int myAtoi(String str) {
        char[] arr = str.toCharArray();
        int rev = 0, index, n = arr.length;
        int flag = 1;//1是正，-1为负
        for (index = 0; index < n; index++) {
            //跳过空格
            if (arr[index] == ' ') continue;
            //找到正负号，判断负号后是否有数字
            if (index <= n - 2 && arr[index] == '-' && Character.isDigit(arr[index + 1])) {
                flag = -1;
                index++;
                break;
            } else if (index <= n - 2 && arr[index] == '+' && Character.isDigit(arr[index + 1])) {
                index++;
                break;
            }
            //找到数字
            else if (Character.isDigit(arr[index])) break;
            //其他字符
            else return 0;
        }
        //循环终止条件：1.越界，2.不是数字
        while (index < n && Character.isDigit(arr[index])) {
            //获取当前位
            int pop = arr[index] - '0';
            //判断正负是否越界
            if (flag == 1 && (rev > Integer.MAX_VALUE / 10 
               || (rev == Integer.MAX_VALUE / 10 && pop > 7)))
                	return 2147483647;
            if (flag == -1 && (rev > -(Integer.MIN_VALUE / 10) 
                || (rev == -(Integer.MIN_VALUE / 10) && pop > 8)))
                	return -2147483648;
            //添加当前位
            rev = rev * 10 + pop;
            index++;
        }
        return rev * flag;
    }
}
```