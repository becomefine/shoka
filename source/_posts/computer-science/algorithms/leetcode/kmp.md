---
title: 详解kmp算法
math: true
date: 2022-02-27 15:53:13
categories:
 - [计算机科学,数据结构与算法,leetcode]
tags: 
 - 数据结构
 - 算法
 - 编程技术
 - leetcode
---

# 什么是KMP算法
`转载`

KMP算法要解决的问题就是在字符串（也叫主串）中的模式（pattern）定位问题。说简单点就是我们平常说的关键字搜索。模式串就是关键字（接下来也称他为P），如果它在一个主串（接下来称为T）中出现，就返回它的具体位置，否则返回-1（常用手段）。
![kmp1](/assets/algorithms/KMP1.png)
首先，对于这个问题有一个很单纯的想法：从左到右一个个匹配，如果这个过程中有某个字符不匹配，就跳回去，将模式串向右移动一位。这有什么难的？
我们可以这样初始化：
![kmp2](/assets/algorithms/KMP2.png)
之后我们只需要比较i指针指向的字符和j指针指向的字符是否一致。如果一致就都向后移动，如果不一致，如下图：
![kmp3](/assets/algorithms/KMP3.png)
A和E不相等，那就把i指针移回第一位（假设下标从0开始），j移动到模式串的第0位，然后由重新开始这个步骤：
![kmp4](/assets/algorithms/KMP4.png)
基于这个想法我们可以得到以下的程序：
```java
/**
 * 暴力破解法
 * @param ts 主串
 * @param ps 模式串
 * @return 如果找到，返回在主串中第一个字符出现的下标，否则为-1
 */
public static int bf(String ts, String ps) {
    char[] t = ts.toCharArray();
    char[] p = ps.toCharArray();
    int i = 0; // 主串的位置
    int j = 0; // 模式串的位置
    while (i < t.length && j < p.length) {
       if (t[i] == p[j]) { // 当两个字符相同，就比较下一个
           i++;
           j++;
       } else {
           i = i - j + 1; // 一旦不匹配，i后退
           j = 0; // j归0
       }
    }
    if (j == p.length) {
       return i - j;
    } else {
       return -1;
    }
}
```
如果认为来寻找的话，肯定不会再把i移动回第1位，因为主串匹配失败的位置前面除了第一个A之外再也没有A了，我们为什么能知道主串前面只有一个A？因为我们已经知道前面三个字符都是匹配的！（这很重要）。移动过去肯定也是不匹配的！有一个想法，i可以不动，我们只需要移动j即可，如下图：
![kmp5](/assets/algorithms/KMP5.png)
上面的这种情况还是比较理想的情况，我们最多也就多比较了两次。但假如是在主串“SSSSSSSSA”中查找“SSSB”，比较到最后一个才知道不匹配，然后回溯，这个的效率显然是最低的。
大牛们是无法忍受“暴力破解”这种低效的手段的，于是研究除了KMP算法。其思想就如同我们上边所看到的一样：“利用已经部分匹配这个有效信息，保持i指针不回溯，通过修改j指针，让模式串尽量地移动到有效的位置。
所以，整个KMP的重点就在于当某一个字符与主串不匹配时，我们应该知道j指针要移动到哪？
接下来我们自己来发现j的移动规律：
![kmp6](/assets/algorithms/KMP6.png)
如图：C和D不匹配了，我们要把j移动到哪？显然是第1位。为什么？因为前面有一个A相同啊：
![kmp7](/assets/algorithms/KMP7.png)
如下图也是一样的情况：
![kmp8](/assets/algorithms/KMP8.png)
可以把j指针移动到第2位，因为前面有两个字母是一样的：
![kmp9](/assets/algorithms/KMP9.png)
至此我们可以大概看出一点端倪，当匹配失败时，j要移动的下一个位置k。存在着这样的性质：**最前面的k个字符和j之前最后k个字符是一样的**。
如果用数学公式来表示是这样的
$$\begin {array}{c}
P[0 ~ k-1] == P[j-k ~ j-1]
\end {array}$$
至此我们可以大概看出一点端倪，当匹配失败时，j要移动的下一个位置k。存在着这样的性质：**最前面的k个字符和j之前的最后k个字符是一样的**。

这个相当重要，如果觉得不好记的话，可以通过下图来理解：
![kmp10](/assets/algorithms/KMP10.png)
弄明白了这个就应该可能明白为什么可以直接将j移动到k位置了。
因为：

**当T[i] != P[j]时**
**有T[i-j ~ i-1] == P[0 ~ j-1]**
**由P[0 ~ k-1] == P[j-k ~ j-1]**
**必然：T[i-k ~ i-1] == P[0 ~ k-1]**


上一段证明了我们为什么可以直接将j移动到k而无须再比较前面的k个字符。

接下来就是重点，怎么求这个k呢？因为在P的每一个位置都可能发生不匹配，也就是说我们要计算每一个位子j对应的k，所以用一个数组next来保存，**next[j] = k, 表示当T[i] != P[j] 时，j指针的下一个位置**。

代码如下：
```java
public static int [] getNext(String ps){
    char [] p = ps.toCharArray();
    int[] next = new int3[p.length];
    next[0] = -1;
    int j = 0;
    int k = -1;
    while(j < p.length -1){
        if(k == -1 || p[j] == p[k]){
            next[++j] = ++k;
        }else{
            k = next [k];
        }
    }
    return next;
}
```
推导如下：
记住一点，**next[j]的值（也就是k）表示，当P[j] != T[i] 时，j指针的下一步移动位置**。
先来看第一个：当j为0是，如果这时候不匹配，怎么办？
![kmp11](/assets/algorithms/KMP11.png)
像上图这种情况，j已经在最左边了，不可能再移动了，这时候要应该是i指针后移。所以在代码中才会有next[0] = -1，这个初始化。
如果是当j为1的时候呢？
![kmp12](/assets/algorithms/KMP12.png)
显然，**j指针一定是后移到0位置的**。因为它前面也就只有这一个位置了~~~
下面这个是最重要的，请看如下图：
![kmp13](/assets/algorithms/KMP13.png)
![kmp14](/assets/algorithms/KMP14.png)

仔细对比两个图。可以发现：
$$\begin {array}{c}
当P[k] == P[j]时
有next[j+1] == next[j] + 1
\end {array}$$

可以证明：
因为在P[j]之前已经有P[0 ~ k-1] == p[j-k ~ j-1]。（next[j] == k），这时候现有P[k] == P[j]，我们是不是可以得到P[0 ~ k-1] + P[k] == P[j-k ~ j-1] + P[j]。
即： P[0 ~ k] == P[j-k ~ j]，即next[j+1] == next[j] + 1 。

具体如下图：
如果P[k] != P[j]:
![kmp15](/assets/algorithms/KMP15.png)

像这种情况，如果从代码上看应该是这一句：k = next[k]; 为什么是这样子？看下面：
![kmp16](/assets/algorithms/KMP16.png)

现在知道为什么要 k = next[k]了吧！像上边的例子，我们已经不可能找到[A, B, A, B]这个最长的后缀串了，但我们还是可能找到[A, B]、[B]这样的前缀串的。所以这个过程像不像在定位[A, B, A, C]这个串，当C和主串不一样了（也就是k位置不一样了），那当然是把指针移动到next[k]啦。

有了next数组之后一切好办，KMP算法如下：
```java
public static int KMP(String ts, String ps){
    char[] t = ts.toCharArray();
    char[] p = ps.toCharArray();
    int i = 0; //主串位置
    int j = 0; //模式串位置
    int[] next = getNext(ps);
    while(i<t.length && j < p.length){
        if(j == -1 || t[i] == p[j]){
            //当j为-1时，要移动的是i，当然j也要归0
            i++;
            j++;
        }else{
            //i不需要回溯了
            //i = i - j + 1
            j = next[j];
        }
    }
    if(j == p.length){
        return i - j;
    }else{
        return -1;
    }
}
```
和暴力破解相比，就改动了4个地方。其中最主要的一点就是，i不需要回溯了。
最后，来看一下上边的算法存在的缺陷。来看第一个例子：
![kmp17](/assets/algorithms/KMP17.png)

显然，当我们上边的算法得到的next数组应该是[-1, 0, 0, 1]
所以下一步我们应该是把j移动到第一个元素格：
![kmp18](/assets/algorithms/KMP18.png)

不难发现，这一步是完全没有意义的，因为后面的B已经已经不匹配了，那前面的B也一定是不匹配的，同样的情况其实还发生在第2个元素A上。
显然，发生问题的原因在于P[j] == P[next[j]]
所以我们也只需要添加一个判断条件即可：
```java
public static int[] getNext(String ps){
    char[] p = ps.toCharArray();
    int[] next = new int[p.lenght];
    next[0] = -1;
    int j = 0;
    int k = -1;
    while(j < p.length - 1){
        if(p[++j] == p[++k]){
            next[j] == next[k];
        }else{
            next[j] = k;
        }
    } else{
        k = next[k];
    }
    return next;
}
