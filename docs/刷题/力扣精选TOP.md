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

## 10. 正则表达式匹配

给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。

*   '.' 匹配任意单个字符

*   '*' 匹配零个或多个前面的那一个元素

所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。

说明:

*   s 可能为空，且只包含从 a-z 的小写字母。
*   p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。

>   示例 1:
>
>   输入:
>
>   s = "aa"
>
>   p = "a"
>
>   输出: false
>
>   解释: "a" 无法匹配 "aa" 整个字符串。
>
>   示例 2:
>
>   输入:
>
>   s = "aa"
>
>   p = "a*"
>
>   输出: true
>
>   解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
>
>   示例 3:
>
>   输入:
>
>   s = "ab"
>
>   p = ".*"
>
>   输出: true
>
>   解释: “.\*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
>
>   示例 4:
>
>   输入:
>
>   s = "aab"
>
>   p = “c\*a*b"
>
>   输出: true
>
>   解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
>
>   示例 5:
>
>   输入:
>   s = "mississippi"
>
>   p = “mis\*is\*p*."
>
>   输出: false
>

```java
class Solution {
    public boolean isMatch(String s, String p) {
       /*
       s和p可能为空。空的长度就是0，但是这些情况都已经判断过了，只需要判断是否为null即可
       if(p.length()==0&&s.length()==0)
            return true;
            */
        if(s==null||p==null)
            return false;
       int rows = s.length();
       int columns = p.length();
       boolean[][]dp = new boolean[rows+1][columns+1];
       //s和p两个都为空，肯定是可以匹配的，同时这里取true的原因是
       //当s=a，p=a，那么dp[1][1] = dp[0][0]。因此dp[0][0]必须为true。
       dp[0][0] = true;
        for(int j=1;j<=columns;j++)
        {   
            //p[j-1]为*可以把j-2和j-1处的字符删去，只有[0,j-3]都为true才可以
            //因此dp[j-2]也要为true，才可以说明前j个为true
            if(p.charAt(j-1)=='*'&&dp[0][j-2])
                dp[0][j] = true;
        }

        for(int i=1;i<=rows;i++)
        {
            for(int j=1;j<=columns;j++)
            {
                char nows = s.charAt(i-1);
                char nowp = p.charAt(j-1);
                if(nows==nowp)
                {
                    dp[i][j] = dp[i-1][j-1];
                }else{
                    if(nowp=='.')
                        dp[i][j] = dp[i-1][j-1];
                    else if(nowp=='*')
                    {
                        //p需要能前移1个。（当前p指向的是j-1，前移1位就是j-2，因此为j>=2）
                        if(j>=2){
                            char nowpLast = p.charAt(j-2);
                            //只有p[j-2]==s[i-1]或p[j-2]==‘.’才可以让*取1个或者多个字符：
                            if(nowpLast==nows||nowpLast=='.')
                                dp[i][j] = dp[i-1][j]||dp[i][j-1];
                            //不论p[j-2]是否等于s[i-1]都可以删除掉j-1和j-2处字符：
                            dp[i][j] = dp[i][j]||dp[i][j-2];
                        }
                    }
                    else
                        dp[i][j] = false;
                }
            }
        }
        return dp[rows][columns];
    }
}
```

## 13. 罗马数字转整数

罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

*   I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
*   X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
*   C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

>   示例 1:
>
>   输入: "III"
>
>   输出: 3
>
>   示例 2:
>
>   输入: "IV"
>
>   输出: 4
>
>   示例 3:
>
>   输入: "IX"
>
>   输出: 9
>
>   示例 4:
>
>   输入: "LVIII"
>
>   输出: 58
>
>   解释: L = 50, V= 5, III = 3.
>
>   示例 5:
>
>   输入: "MCMXCIV"
>
>   输出: 1994
>
>   解释: M = 1000, CM = 900, XC = 90, IV = 4.

```java
class Solution {
    public int romanToInt(String s) {
        char[] ch = s.toCharArray();
        int num = 0;
        for (int i = 0; i < ch.length - 1; i++) {
            if(ch[i] == 'I' && (ch[i + 1] == 'V' || ch[i + 1] == 'X'))
                num -= 2;
            if(ch[i] == 'X' && (ch[i + 1] == 'L' || ch[i + 1] == 'C'))
                num -= 20;
            if(ch[i] == 'C' && (ch[i + 1] == 'D' || ch[i + 1] == 'M'))
                num -= 200;
        }
        for (int i = 0; i < ch.length; i++) {
            switch (ch[i]) {
                case 'M': {
                    num += 1000;
                    continue;
                }
                case 'D': {
                    num += 500;
                    continue;
                }
                case 'C': {
                    num += 100;
                    continue;
                }
                case 'L': {
                    num += 50;
                    continue;
                }
                case 'X': {
                    num += 10;
                    continue;
                }
                case 'V': {
                    num += 5;
                    continue;
                }
                default: {
                    num += 1;
                    continue;
                }
            }
        }
        return num;
    }
}
```

## 14. 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

>   示例 1:
>
>   输入: ["flower","flow","flight"]
>
>   输出: "fl"
>
>   示例 2:
>
>   输入: ["dog","racecar","car"]
>
>   输出: ""
>
>   解释: 输入不存在公共前缀。
>
>   说明:
>
>   所有输入只包含小写字母 a-z 。
>

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length == 0) return "";
        
        //令最长公共前缀 ans 的值为第一个字符串，进行初始化
        String ans = strs[0];
        
        //遍历后面的字符串，依次将其与 ans 进行比较，两两找出公共前缀，最终结果即为最长公共前缀
        for(int i = 1; i < strs.length; i++) {
            int j = 0;
            for(; j < ans.length() && j < strs[i].length(); j++) {
                if(ans.charAt(j) != strs[i].charAt(j))
                    break;
            }
            //更新当前最长公共前缀
            ans = ans.substring(0, j);
            //无相同前缀，返回""
            if(ans.equals(""))
                return ans;
        }
        
        return ans;
    }
}
```

## 15. 三数之和

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

>   示例：
>
>   给定数组 nums = [-1, 0, 1, 2, -1, -4]，
>
>   满足要求的三元组集合为：
>
>   ```
>   [
>     [-1, 0, 1],
>     [-1, -1, 2]
>   ]
>   ```

```java
class Solution {
    public static List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans = new ArrayList();
        if(nums == null || nums.length < 3) return ans;
        int len = nums.length;
        // 排序
        Arrays.sort(nums); 
        for (int i = 0; i < len ; i++) {
            // 如果当前数字大于0，则三数之和一定大于0，所以结束循环
            if(nums[i] > 0) break; 
            // 如果nums[i] == nums[i−1]，则说明该数字重复，会导致结果重复，所以应该跳过
            if(i > 0 && nums[i] == nums[i - 1]) continue;
            
            int L = i + 1;
            int R = len - 1;
            while(L < R){
                int sum = nums[i] + nums[L] + nums[R];
                if(sum == 0){
                    ans.add(Arrays.asList(nums[i],nums[L],nums[R]));
                    //nums[L] == nums[L+1] 则会导致结果重复，应该跳过，L++
                    while (L < R && nums[L] == nums[L + 1]) L++; 
                    //nums[R] == nums[R−1] 则会导致结果重复，应该跳过，R--
                    while (L < R && nums[R] == nums[R - 1]) R--;
                    L++;
                    R--;
                }
                else if (sum < 0) L++;
                else if (sum > 0) R--;
            }
        }        
        return ans;
    }
}
```

## 17. 电话号码的字母组合

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gfxgnvzdqxj30dv0ckdhp.jpg" alt="img" style="zoom: 50%;" />

>   示例:
>
>   输入："23"
>
>   输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
>
>   说明:
>
>   尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

```java
/*
解题思路：
1. digits 是数字字符串
2. s(digits) 是 digits 所能代表的字母字符串
3. 把 s(digits) 分解 可表示为
    s(digits[0...n-1])
    = letter(digits[0] + digits[1...n-1])
    = letter(digits[0] + letter(digits[1] + digits[2...n-1])
    = .....
*/
class Solution {
    Map<String, String> phone = new HashMap<String, String>() {{
        put("2", "abc");
        put("3", "def");
        put("4", "ghi");
        put("5", "jkl");
        put("6", "mno");
        put("7", "pqrs");
        put("8", "tuv");
        put("9", "wxyz");
      }};
    
    List<String> output = new ArrayList<String>();
    
    private void backtrack(String combination, String next_digits) {
        if (next_digits.length() == 0){
            output.add(combination);
        }else {
            String digit = next_digits.substring(0, 1);
            String letters = phone.get(digit);
            for (int i = 0; i < letters.length(); i++) {
                String letter = letters.substring(i, i + 1);
                backtrack(combination + letter, next_digits.substring(1));
            }

        }
    }
    
    public List<String> letterCombinations(String digits) {
        if (digits.length() != 0)
          backtrack("", digits);
        return output;
    }
}
```

## 19. 删除链表的倒数第N个节点

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

>   示例：
>
>   给定一个链表: 1->2->3->4->5, 和 n = 2.
>
>   当删除了倒数第二个节点后，链表变为 1->2->3->5.
>
>   说明：
>
>   给定的 n 保证是有效的。
>
>   进阶：
>
>   你能尝试使用一趟扫描实现吗？
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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null || head.next == null) {
            return null;
        }
        ListNode res = new ListNode(-1);
        res.next = head;
        ListNode fast = res;
        ListNode slow = res;

        while (n > 0) {
            fast = fast.next;
            n--;
        }
        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;

        return res.next;
    }
}
```

## 20. 有效的括号

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。

左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

>   示例 1:
>
>   输入: "()"
>
>   输出: true
>
>   示例 2:
>
>   输入: "()[]{}"
>
>   输出: true
>
>   示例 3:
>
>   输入: "(]"
>
>   输出: false
>
>   示例 4:
>
>   输入: "([)]"
>
>   输出: false
>
>   示例 5:
>
>   输入: "{[]}"
>
>   输出: true

```java
class Solution {
    public boolean isValid(String s) {
        if (s.length() % 2 == 1) // 奇数个字符的字符串 显然无法完成括号匹配
            return false;
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char theChar = s.charAt(i);
            if (theChar == '(' || theChar == '{' || theChar == '[')
                stack.push(theChar);
            else {
                if (stack.empty()) // 栈中已无左括号，此时字符为右括号，无法匹配。
                    return false;
                char preChar = stack.peek();
                if ((preChar == '{' && theChar == '}') 
                || (preChar == '(' && theChar == ')') 
                || (preChar == '[' && theChar == ']'))
                    stack.pop();
                else return false;
            }
        }
        return stack.empty();
    }
}
```

## 21. 合并两个有序链表

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。  

>   示例：
>
>   输入：1->2->4, 1->3->4
>
>   输出：1->1->2->3->4->4

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        } else if (l2 == null) {
            return l1;
        }
        ListNode dummy = new ListNode(0);
        ListNode curr  = dummy;

        // 如果两个对应的链表都没有走完
        while(l1!=null && l2!=null){
            // 判断l1和l2大小
            if (l1.val < l2.val){
                // l1 小，curr走向l1
                curr.next = l1;
                // l1 向后走一位
                l1 = l1.next;
            }else {
                // l2 小 ， curr走向l2
                curr.next = l2;
                // l2向后走一位
                l2 = l2.next;
            }
            // curr记录下已经被选出大小的节点
            curr = curr.next;
        }

        if (l1 == null) curr.next = l2;
        if (l2 == null) curr.next = l1;

        return dummy.next;
    }
}
```

## 22. 括号生成

数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

>   示例：
>
>   ```
>   输入：n = 3
>   
>   输出：[
>          "((()))",
>          "(()())",
>          "(())()",
>          "()(())",
>          "()()()"
>        ]
>   ```

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new LinkedList<>();
        //总共需要生成2n个字符的数组（左括号n个，右括号n个）
        backtrack(0, 0, new char[2 * n], result);
        return result;
    }

    //track为当前选择的组合，left为组合中左括号数量，right为组合中右括号数量
    public void backtrack(int left, int right, char[] track, List<String> result) {
        if (left + right == track.length) {
            //如果左括号数量等于右括号数量，并且长度达到要求，则是一个合法的结果，将其加入结果集
            if (left == right) {
                result.add(new String(track));
            }
            return;
        }
        //如果左括号数量小于右括号数量，不合法
        if (left < right) {
            return;
        }
        //每个位置有两种情况：1.左括号 2.右括号 递归列举所有的情况
        //选择左括号作为当前元素
        track[left + right] = '(';
        backtrack(left + 1, right, track, result);
        //选择右括号作为当前元素
        track[left + right] = ')';
        backtrack(left, right + 1, track, result);
    }
}
```

## 26. 删除排序数组中的重复项

给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。 

>   示例 1:
>
>   给定数组 nums = [1,1,2], 
>
>   函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
>
>   你不需要考虑数组中超出新长度后面的元素。
>
>   示例 2:
>
>   给定 nums = [0,0,1,1,1,2,2,3,3,4],
>
>   函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
>
>   你不需要考虑数组中超出新长度后面的元素。
>

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums == null || nums.length == 0) return 0;
        int p = 0;
        int q = 1;
        while(q < nums.length){
            //如果不相等，将 q 位置的元素复制到 p+1 位置上，p 后移一位，q 后移 1 位
            if(nums[p] != nums[q]){
                //当 q - p > 1 时，才进行复制。优化数组中没有重复元素的情况
                if(q - p > 1){
                    nums[p + 1] = nums[q];
                }
                p++;
                q++;
            }
            //如果相等，q 后移 1 位
            else {
                q++;
            }
        }
        //返回 p + 1，即为新数组长度。
        return p + 1;
    }
}
```

