# 8-JavaNode-Array

[toc]

## labuladong 双指针和数组

双指针技巧主要分为两类：**左右指针**和**快慢指针**。

所谓左右指针，就是两个指针相向而行或者相背而行；而所谓快慢指针，就是两个指针同向而行，一快一慢。

对于单链表来说，大部分技巧都属于快慢指针。

在数组中并没有真正意义上的指针，但我们可以把索引当做数组中的指针，这样也可以在数组中施展双指针技巧，**本文主要讲数组相关的双指针算法**。

### 快慢指针 去重数组 去特定值

经典题目，要求原地弄

```java
int removeDuplicates(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    int slow = 0, fast = 0;
    while (fast < nums.length) {
        if (nums[fast] != nums[slow]) {
            slow++;
            // 维护 nums[0..slow] 无重复
            nums[slow] = nums[fast];
        }
        fast++;
    }
    // 数组长度为索引 + 1
    return slow + 1;
}
```

维护 nums[0..slow] 无重复

如果是一样的，就 fast 往下走，如果不一样，就把 slow 的下一个 assign 成 fast，然后 fast 再往下走

链表去重，几乎完全一样的操作，把操作数组改为操作链表

还能解决：

* 去掉特定元素

  * 如果 fast != val, assign fast to slow ,两个都往下走，如果等于，只有 fast 往下走
  * 维护 [0-slow) 都是没 val的，也就是 slow 指的是什么样的是不确定的

* 把 0 都移到末尾

  * 维护 [0-slow) 都是没 0 的

  * 或者直接用之前的结果 

    * ```java
      void moveZeroes(int[] nums) {
          // 去除 nums 中的所有 0，返回不含 0 的数组长度
          int p = removeElement(nums, 0);
          // 将 nums[p..] 的元素赋值为 0
          for (; p < nums.length; p++) {
              nums[p] = 0;
          }
      }
      
      // 见上文代码实现
      int removeElement(int[] nums, int val);
      ```

### 左右指针

* 二分查找是左右指针的一种应用
* 只要数组有序，就是双指针，二分查找

2 sum 经典左右指针，从两边往中间缩

* 翻转数组，字符串，也可以左右指针
* 判断回文



* 找最长回文串

  * 从中心像两侧扩展的双指针

  * ```java
    // 在 s 中寻找以 s[l] 和 s[r] 为中心的最长回文串
    String palindrome(String s, int l, int r) {
        // 防止索引越界
        while (l >= 0 && r < s.length()
                && s.charAt(l) == s.charAt(r)) {
            // 双指针，向两边展开
            l--; r++;
        }
        // 返回以 s[l] 和 s[r] 为中心的最长回文串
        return s.substring(l + 1, r);
    }
    
    // 这样，如果输入相同的 l 和 r，就相当于寻找长度为奇数的回文串，如果输入相邻的 l 和 r，则相当于寻找长度为偶数的回文串。
    ```

  * 然后再遍历

  * ```java
    for 0 <= i < len(s):
        找到以 s[i] 为中心的回文串
        找到以 s[i] 和 s[i+1] 为中心的回文串
        更新答案
    ```

  * 好像还是 n2，留个悬念