# KMP

- 作用：**KMP主要应用在字符串匹配上**

- KMP的主要思想：**当出现字符串不匹配时，可以知道一部分之前已经匹配的文本内容，可以利用这些信息避免从头再去做匹配了**

- **next数组**就是一个前缀表（prefix table），是用来回退的，它记录了模式串与主串(文本串)不匹配的时候，模式串应该从哪里开始重新匹配



# next[i] 数组

记录下标i之前（包括i）的字符串（**模式串**）中，**最长相同前缀后缀**的长度 

**前缀**：是指不包含最后一个字符的所有以第一个字符开头的连续子串

**后缀**：是指不包含第一个字符的所有以最后一个字符结尾的连续子串

eg.

a 的最长相等前后缀为0
aa 的最长相等前后缀为1
aaa 的最长相等前后缀为2



# 如何计算前缀表

接下来就要说一说怎么计算前缀表。

如图：

<img src='https://code-thinking.cdn.bcebos.com/pics/KMP%E7%B2%BE%E8%AE%B25.png' width=600 alt='KMP精讲5'> </img></div>

长度为前1个字符的子串`a`，最长相同前后缀的长度为0。（注意字符串的**前缀是指不包含最后一个字符的所有以第一个字符开头的连续子串**；**后缀是指不包含第一个字符的所有以最后一个字符结尾的连续子串**。）

<img src='https://code-thinking.cdn.bcebos.com/pics/KMP%E7%B2%BE%E8%AE%B26.png' width=600 alt='KMP精讲6'> </img></div>
长度为前2个字符的子串`aa`，最长相同前后缀的长度为1。

<img src='https://code-thinking.cdn.bcebos.com/pics/KMP%E7%B2%BE%E8%AE%B27.png' width=600 alt='KMP精讲7'> </img></div>
长度为前3个字符的子串`aab`，最长相同前后缀的长度为0。

以此类推：
长度为前4个字符的子串`aaba`，最长相同前后缀的长度为1。
长度为前5个字符的子串`aabaa`，最长相同前后缀的长度为2。
长度为前6个字符的子串`aabaaf`，最长相同前后缀的长度为0。

那么把求得的最长相同前后缀的长度就是对应前缀表的元素，如图：
<img src='https://code-thinking.cdn.bcebos.com/pics/KMP%E7%B2%BE%E8%AE%B28.png' width=600 alt='KMP精讲8'> </img></div>

可以看出模式串与前缀表对应位置的数字表示的就是：**下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀。**

再来看一下如何利用 前缀表找到 当字符不匹配的时候应该指针应该移动的位置。如动画所示：

![KMP精讲2](https://code-thinking.cdn.bcebos.com/gifs/KMP%E7%B2%BE%E8%AE%B22.gif)

找到的不匹配的位置， 那么此时我们要看它的**前一个字符**的前缀表的数值是多少









# 步骤

## 1.通过模式串构建 **next** 数组

通过前缀表构建，将前缀表中的值一一对应的填进next，前缀表中的值表示：记录下标i之前（包括i）的字符串（**模式串**）中，**最长相同前缀后缀**的长度

有些地方从 j = -1 开始（减一和不减一都可以实现，只是实现的方式不一样）

```java
//不进行减一
int j = 0;
next[0] = 0;
for(int i = 1; i < s.size(); i++) { // 注意i从1开始
    while (j > 0 && s[i] != s[j + 1]) { // 前后缀不相同了
        j = next[j - 1]; // 向前回退
    }
    if (s[i] == s[j]) { // 找到相同的前后缀
        j++;
    }
    next[i] = j; // 将j（前缀的长度）赋给next[i]
}
```



## 2.使用 next 数组来匹配主串

文本串：s

模式串：t

定义两个下标：

- j 指向模式串起始位置

- i 指向文本串起始位置



i 就从0开始，遍历文本串，**进行  s[i] 与 t[j] d的比较：**

- 相同：i、j 同时++
- 不相同：j 回退，**j = next[ j - 1]**



如果 **j == needle.size()**，说明模式串 t 完全匹配文本串 s 里的某个子串了（true）



否则为 false



# 例题

[686. 重复叠加字符串匹配](https://leetcode-cn.com/problems/repeated-string-match/)

给定两个字符串 `a` 和 `b`，寻找重复叠加字符串 `a` 的最小次数，使得字符串 `b` 成为叠加后的字符串 `a` 的子串，如果不存在则返回 `-1`。

**注意：**字符串 `"abc"` 重复叠加 0 次是 `""`，重复叠加 1 次是 `"abc"`，重复叠加 2 次是 `"abcabc"`。

思路：假设a为abc，b的为cabcabca，这时候b/a=2,就是最少要放两个a,可能a能包含b【此例子不成立，加上后面的条件才能得出答案，这一步只做粗筛选】

- 如果刚好取的是完整的a字符串，那么，直接取b/a这个长度就可以了，例子：abc abcabc,此时取2；
- 如果前面取的是不完整的a字符串，如abc cabcabc,这时候除完要加一个；同理，abc abcabca也要加一个，取3；
- 如果前后都是不完整的a字符串，如abc cabcabca,则前后都加一个才能拿到b,此时取4；

综上，只有b/a，b/a+1,b/a+2这三个数可能是正确答案，如果三个数都找不到，那就没有答案了。

```java
class Solution {
    public int repeatedStringMatch(String a, String b) {
            int min = b.length()/a.length();
                        StringBuilder sb = new StringBuilder();
                        for(int i = 1; i <= min+2; i++){
                            sb.append(a);
                            if(i>=min && isKMP(sb.toString(),b)){
                                return i;
                            }
                        }
            return -1;
    }

    public boolean isKMP(String s,String t){
        if(s.length()<t.length()){
            return false;
        }
        int[] next = new int[t.length()];
        int j = 0;
        next[0] = 0;
        for(int i =1;i<t.length();i++){
            while(j>0&&t.charAt(i)!=t.charAt(j)){
                j = next[j-1];
            }
            if(t.charAt(i)==t.charAt(j)){
                j++;
            }
            next[i] = j;
        }

        j = 0;
        for(int i =0;i<s.length();i++){
            while(j>0&&s.charAt(i)!=t.charAt(j)){
                j = next[j-1];
            }
            if(s.charAt(i)==t.charAt(j)){
                j++;
            }
            if(j == t.length()){
                return true;
            }
        }
        return false;
    }
}
```









