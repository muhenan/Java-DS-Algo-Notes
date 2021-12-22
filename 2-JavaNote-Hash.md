[toc]

# Hash

主要就是介绍 HashMap 和 HashSet

HashMap and HashSet both are one of the most important classes of Java Collection framework.

Following are the important differences between HashMap and HashSet.

| Sr. No. |           Key           |                           HashMap                            |                           HashSet                            |
| ------- | :---------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 1       |     Implementation      |       Hashmap is the implementation of Map interface.        | Hashset on other hand is the implementation of set interface. |
| 2       | Internal implementation | Hashmap internally do not implements hashset or any set for its implementation. |   Hashset internally uses Hashmap for its implementation.    |
| 3       |   Storage of elements   | HashMap Stores elements in form of key-value pair i.e each element has its corresponding key which is required for its retrieval during iteration. | HashSet stores only objects no such key value pairs maintained. |
| 4       |  Method to add element  |  Put method of hash map is used to add element in hashmap.   | On other hand add method of hashset is used to add element in hashset. |
| 5       |    Index performance    | Hashmap due to its unique key is faster in retrieval of element during its iteration. | HashSet is completely based on object so compared to hashmap is **slower**. |
| 6       |      Null Allowed       | Single null key and any number of null value can be inserted in hashmap without any restriction. | On other hand Hashset allows only one null value in its collection,after which no null value is allowed to be added. |

Map 更快，Set 其实是封装了 Map。

* Java 中的 **HashSet** 用于存储一个集合，可以查找元素是否在集合中。
  * 如果元素有穷，并且范围不大，那么可以用一个布尔数组来存储一个元素是否存在。例如对于只有小写字符的元素，就可以用一个长度为 26 的布尔数组来存储一个字符集合，使得空间复杂度降低为 O(1)。
* Java 中的 **HashMap** 主要用于映射关系，从而把两个元素联系起来。HashMap 也可以用来对元素进行计数统计，此时键为元素，值为计数。和 HashSet 类似，如果元素有穷并且范围不大，可以用整型数组来进行统计。在对一个内容进行压缩或者其它转换时，利用 HashMap 可以把原始内容和转换后的内容联系起来。



Hash Table 很多时候做的事都和 **计数** 有关



## 数据结构

### HashMap

实现原理：https://www.cnblogs.com/chengxiao/p/6059914.html

存储、查找复杂度都是 O(1) 就完事了



## 算法

### 1. Tow Sum E

 Leetcode 1. Two Sum E
 经典中的经典，热门经典题，hash 经典题
 提供了三种解法：

1. 暴力 n2 空间 1
     双循环，找和符合条件的两个元素
2. 排序的 nlogn 空间 1
先排序，再双指针
可以先对数组进行排序，然后使用双指针方法或者二分查找方法。这样做的时间复杂度为 O(NlogN)，空间复杂度为 O(1)

3. hash n 空间 n
一放一查
拿时间换空间了

用了Java强大的容器，一个哈希表，key 是 值，value 是 index，每一个都这样存下来，到下一个是找是不是有对应的那个值，找到就返回相应的index

```java
		public int[] twoSum3(int[] nums, int target) {
        HashMap<Integer, Integer> indexForNum = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (indexForNum.containsKey(target - nums[i])) {
                return new int[]{indexForNum.get(target - nums[i]), i};
            } else {
                indexForNum.put(nums[i], i);
            }
        }
        return null;
    }
```

Java 数组的初始化：https://www.php.cn/java/base/461678.html



### 217. Contains Duplicate E

HashSet 的经典应用

一个数组，找是否有重复的数，直接一个 HashSet 看最后是不是少了即可

复杂度 N， N

// 关于涉及排序的方法，看力扣官方题解吧

```java
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }
        return set.size() < nums.length;
    }
```



### 594. Longest Harmonious Subsequence (Easy)

```
Input: [1,3,2,2,5,2,3,7]
Output: 5
Explanation: The longest harmonious subsequence is [3,2,2,2,3].
```

和谐序列中最大数和最小数之差正好为 1，应该注意的是序列的元素不一定是数组的连续元素。



关于**子序列 Subsequence**：

* 子序列可以是不连续的
* 子序列有关的题，一定要有**整体**的思想，能够从一个数组中跳出来，从整体大的角度抽丝剥茧，上帝视角看问题



hash与计数：

* hashtable很多时候应用起来都和**计数**有关！

  * 直接 hashmap 计数
  * 弄一个 26 长的 int[] 计数，都是这个意思

  

```java
public int findLHS(int[] nums) {
    Map<Integer, Integer> countForNum = new HashMap<>();
    for (int num : nums) {
        countForNum.put(num, countForNum.getOrDefault(num, 0) + 1);
    }
    int longest = 0;
    for (int num : countForNum.keySet()) {
        if (countForNum.containsKey(num + 1)) {
            longest = Math.max(longest, countForNum.get(num + 1) + countForNum.get(num));
        }
    }
    return longest;
}
```

这个算法的复杂度也算是 O(n)



Java HashMap 的一些函数：

* keySet()
  * 返回一个代表 key 的 set，这里 set 是**乱序**的，即使有的时候偶尔有顺序，但实际上就是乱序的！
    * set 可以用 增强for 的形式
    * https://blog.csdn.net/qq_30225725/article/details/88016602
* getOrDefault(num, 0)
  * 通过一个 key，找 value，找不到就放一个默认的



### 128. Longest Consecutive Sequence (Medium)

Sequence：

* 所有提到 **Sequence（序列） **的题，一定要有**整体**的思想，打散的思想！！！！整体法！！高中竞赛，整体法！！！

```
Given [100, 4, 200, 1, 3, 2],
The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.
```

要求以 O(N) 的时间复杂度求解。

O(n)：

* 只要想达到O(N)，一定是在这个数组上单次的走，或者走有限次数，一定是这样走！关键就在于不重复走！！！

```java
    public int longestConsecutive(int[] nums) {
        HashMap<Integer, Integer> my_map = new HashMap<>();
        for (int num: nums) {
            my_map.put(num, null);
        }
        int longest = 0;
        for (int num: nums) {
            if(!my_map.containsKey(num - 1)) {
                int current = num;
                int current_longest = 1;
                while (my_map.containsKey(current + 1)) {
                    current ++;
                    current_longest ++;
                }
                longest = Math.max(current_longest, longest);
            }
        }
        return longest;
    }
```

**力扣：**

我们考虑枚举数组中的每个数 xx，考虑以其为起点，不断尝试匹配 x+1, x+2, \cdotsx+1,x+2,⋯ 是否存在，假设最长匹配到了 x+yx+y，那么以 xx 为起点的最长连续序列即为 x, x+1, x+2, \cdots, x+yx,x+1,x+2,⋯,x+y，其长度为 y+1y+1，我们不断枚举并更新答案即可。

对于匹配的过程，暴力的方法是 O(n)遍历数组去看是否存在这个数，但其实更高效的方法是用一个哈希表存储数组中的数，这样查看一个数是否存在即能优化至 O(1)O(1) 的时间复杂度。

仅仅是这样我们的算法时间复杂度最坏情况下还是会达到 O(n^2)O(n 2 )（即外层需要枚举 O(n)O(n) 个数，内层需要暴力匹配 O(n)O(n) 次），无法满足题目的要求。但仔细分析这个过程，我们会发现其中执行了很多不必要的枚举，如果已知有一个 x, x+1, x+2, \cdots, x+yx,x+1,x+2,⋯,x+y 的连续序列，而我们却重新从 x+1x+1，x+2x+2 或者是 x+yx+y 处开始尝试匹配，那么得到的结果肯定不会优于枚举 xx 为起点的答案，因此我们在外层循环的时候碰到这种情况跳过即可。

那么怎么判断是否跳过呢？由于我们要枚举的数 xx 一定是在数组中不存在前驱数 x-1x−1 的，不然按照上面的分析我们会从 x-1x−1 开始尝试匹配，因此我们每次在哈希表中检查是否存在 x-1x−1 即能判断是否需要跳过了。

**me：**

1. 最基本的想法是，每次以一个数为起点，然后一直去递归的找找找他的下一个，但这样还是造成 n2 的复杂度。
2. 改进一下找的过程，放在 hashmap里，找用 hashmap 找，这样还是相当于从每一个起点都另起了一条数组，比如从 1 会起 1234，从 2 会起 234，还是有重复。
3. 再改进一下从哪里起步，如果这个数的前一个数也有，那么这个数就一定不能是起点了，这种的话干脆就不作为起点找了，这样的话，真正的起点就只有那三个了，就算是三条数组全走完，也是走了一遍，没有重复！！！！

```
199   4   200   1   399   2   398   3
✅   ❌   ❌   ✅  ❌   ❌   ✅   ❌
```

代码里用的 HashMap，因为理论上 HashMap 比 set 更快。



### 535 TinyURL

TinyURL是一种URL简化服务， 比如：当你输入一个URL https://leetcode.com/problems/design-tinyurl 时，它将返回一个简化的URL http://tinyurl.com/4e9iAk.

要求：设计一个 TinyURL 的加密 encode 和解密 decode 的方法。你的加密和解密算法如何设计和运作是没有限制的，你只需要保证一个URL可以被加密成一个TinyURL，并且这个TinyURL可以用解密方法恢复成原本的URL。

```java
		HashMap<Integer, String> url_map = new HashMap<>();
    int index = 0;

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        url_map.put(index, longUrl);
        return "http://tinyurl.com/" + index++;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return url_map.get(Integer.parseInt(shortUrl.replace("http://tinyurl.com/", "")));
    }
```

* 存储方面，HashMap的应用！！
  * HashMap 可以非常好的用于存储一些东西，通过 key，来找到你存的 value，前提有一个大的 hashmap给你用来存东西。
    * 这也可以理解为一种最简单的加密解密算法。东西放在箱子里，用锁锁住，得到钥匙；通过钥匙打开锁，拿到东西。

* 这里注意学习 Java 中字符串和 Integer 的一些函数。
