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

