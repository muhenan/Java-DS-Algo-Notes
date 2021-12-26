[toc]

# String 字符串

用的是 Java 的 String 类（这里也跟着学习一下 Java 的各种字符串操作）。

## 字符串循环移位包含 String Rotate Contain

**编程之美 3.1**

```
s1 = AABCD, s2 = CDAA
Return : true
```

给定两个字符串 s1 和 s2，要求判定 s2 是否能够被 s1 做循环移位得到的字符串包含。

### 解法一

首先说一下编程之美的高端解法：

**s1 进行循环移位的结果是 s1s1 的子字符串**（这句话是精髓），因此只要判断 s2 是否是 s1s1 的子字符串即可。

```java
    // 编程之美，高端方法，简洁方法
    private boolean IsRotateAndContain (String s1, String s2) {
        String s1s1 = s1.concat(s1);
//        System.out.println(s1);
//        System.out.println(s1s1);
        return s1s1.contains(s2);
    }
```

用到了 Java String 类的两个小函数：

* String.concat(s1)：返回一个新的，连接好的字符串
* String.contains(s1)：返回 boolean 是否包含

***

### 解法二

传统做法：

这里涉及了 Rotate 的写法，掌握 rotate 的具体实现细节即可：

```java
// 字符串的 rotate
private String RotateString (String str) {
    int lengOfStr = str.length();
    if (lengOfStr == 0 || lengOfStr == 1) return str;
    String tempStr1 = str.substring(1);
    String tempStr2 = str.substring(0,1);
    return tempStr1.concat(tempStr2);
}

// 传统方法，一次一次的 rotate，然后看每次的结果是否 contain s2
private boolean IsRotateAndContain2 (String s1, String s2) {
    int lengOfS1 = s1.length();
    for (int i = 0; i < lengOfS1; i++) {
        System.out.println(s1);
        if (s1.contains(s2)) return true;
        s1 = RotateString(s1);
    }
    return false;
}
```

这里主要就是用了 Java String 类的 substring 方法：

* substring()，也是返回一个新的字符串，这个字符串是原字符串的子字符串
  * 如果只有一个参数 就是 start index，**左闭**
  * 如果只有两个参数 就是 start index 和 end index，**左闭右开**



## 151. Reverse Words in a String 翻转字符串中的单词

```
s = "I am a student"
Return "student a am I"
```

经典题目。

### 解法一

**自解方法**：

自解方法的复杂度是 O(n) O(n)，是最容易想到的方法，就是把每个单词都拿出来，然后一个一个的放到一个新的字符串里，放的时候是栈的放法，这样就相当于翻转了。编程需要注意一些细节吧。

```java
// 最早想出的 自解方法 都是 O(n)
    public String reverseWords(String s) {
        String res = new String();
        String word = new String();
        int count = s.length();
        boolean inAWord = false;
        for (int i = 0; i < count; i++) {
            if (s.charAt(i) == ' ') {
                if (!inAWord) continue;
                else {
                    res = word + ' ' + res;
                    word = new String("");
                    inAWord = !inAWord;
                }
            } else {
                word += s.charAt(i);
                if (!inAWord) inAWord = !inAWord;
            }
        }
        if (!word.isEmpty()) {
            res = word + ' ' + res;
        }
        return res.substring(0, res.length() - 1);
    }
```

* 这里有个小技巧就是 boolean inAWord 这个 flag 的使用，这个是非常关键的。要学会这种思想来做记录。

***

### 解法二

还是同样的思想，这次从后面找到一个一个单词，然后放好，这次换一下数据结构，用 char[] ，字符串的操作用 StringBuilder。

* 涉及到字符串内部操作比较多的，建议用 StringBuilder

该方法相比第一种，性能有了大幅度提升！

```java
    // 也是传统逻辑，但是是用的 char[] 的数据结构，字符串的操作用了 StringBuilder
        // 时间性能大幅度提升
    public String reverseWords3(String s) {
        char[] charArray = s.toCharArray();
        int left = 0;
        int right = charArray.length - 1;
        StringBuilder sb = new StringBuilder();
        while (charArray[left] == ' ') left++;
        while (charArray[right] == ' ') right--;
        while (left <= right) {
            int startOfWord = right;
            while (startOfWord >= left && charArray[startOfWord] != ' ') startOfWord--;
            for (int i = startOfWord + 1; i <= right; i++) sb.append(charArray[i]);
            if (startOfWord > left) sb.append(' ');
            while (startOfWord >= left && charArray[startOfWord] == ' ') startOfWord--;
            right = startOfWord;
        }
        return sb.toString();
    }
```

***

### 解法三

**原地做法**，字符串，单词，来回翻转

这种做法其实就是大神追求空间复杂度 O(1)，希望能够在原地解题，不占用额外空间，不过这只能在 c++ 时实现，在 Java 等更高级的语言中是没法实现的，总得弄一个新的 String 来装一些东西。

```c++
        // 我把 C++ 的方法写在下面吧
    /*
一下是 leetcode 大神的原地做法
其中一些细节非常值得好好体会
思路是先一个整体的翻转，再逐个翻转单词
操作细节：
    1. 整体翻转
    2. 通过左右两个指针除去左右的空格
    3. 单词逐个翻转，两个指针，找到单词，然后 reverse 函数
    4. 一个精髓之处在于处理中间冗余空格：
        1. 完全原地操作，覆盖冗余空格，tail i 两个指针，妙至毫巅 （上提，向前覆盖）
        2. 覆盖的整个过程不会影响后面，但是前面得到了正确结果
字符串还是需要一些操作的
*/
    string reverseWords(string s) {
        reverse(s.begin(), s.end());                        //整体反转
        int start = 0, end = s.size() - 1;
        while (start < s.size() && s[start] == ' ') start++;//首空格
        while (end >= 0 && s[end] == ' ') end--;            //尾空格
        if (start > end) return "";                         //特殊情况

        for (int r = start; r <= end;) {                    //逐单词反转
            while (s[r] == ' '&& r <= end) r++;
            int l = r;
            while (s[l] != ' '&&l <= end) l++;
            reverse(s.begin() + r, s.begin() + l);
            r = l;
        }

        int tail = start;                                   //处理中间冗余空格
        for (int i = start; i <= end; i++) {
            if (s[i] == ' '&&s[i - 1] == ' ') continue;
            s[tail++] = s[i];
        }
        return s.substr(start, tail - start);
    }
```

***

### 解法四

Java 语言特性，直接用库函数，有关 regex

```java
    public String reverseWords4(String s) {
        s = s.trim();
        List<String> words = Arrays.asList(s.split("\\s+"));
        Collections.reverse(words);
        return String.join(" ", words);
    }
```

这里涉及到的几个数据结构：

* List 表
  * 表用来存这些单词
* Arrays 数组
  * 通过 asList 把一个数组变成一个表
* Collection 
  * 可以翻转表
* String
  * join 把表里的 String 都连接起来

效率介于解法一和二之间