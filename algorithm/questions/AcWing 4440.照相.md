### AcWing 4440.照相

[4440. 照相 - AcWing题库](https://www.acwing.com/problem/content/4443/)

迫切希望在郡县集市上赢得最佳奶牛摄影师的农夫约翰正在尝试为他的 N 头奶牛拍摄一张完美的照片。

农夫约翰拥有两种品种的奶牛：更赛牛（Guernsey）和荷斯坦牛（Holstein）。

为了使他的照片尽可能地艺术，他想把他的奶牛排成一排，使得尽可能多的更赛牛处于队列中的偶数位置（队列中的第一个位置是奇数位置，下一个是偶数位置，以此类推）。

由于他与他的奶牛缺乏有效的沟通，他可以达到目的的唯一方法是让他的奶牛的偶数长的「前缀」进行反转（一个前缀指的是对于某个位置 j，从第一头奶牛到第 j 头奶牛范围内的所有奶牛）。

请计算农夫约翰达到目的所需要的最小反转次数。

#### 输入格式

输入的第一行包含 N 的值。

第二行包含一个长为 N 的字符串，给出初始时所有奶牛从左到右的排列方式。每个 `H` 代表一头荷斯坦牛，每个 `G` 代表一头更赛牛。

#### 输出格式

输出一行，包含达到目的所需要的最小反转次数。

#### 数据范围

2≤N≤2×10^5 , N 为偶数。

#### 输入样例：

```
14
GGGHGHHGHHHGHG
```

#### 输出样例：

```
1
```

#### 样例解释

在这个例子中，只需反转由前六头奶牛组成的前缀即可。

```
   GGGHGHHGHHHGHG （反转前）
-> HGHGGGHGHHHGHG （反转后）
```

在反转之前，四头更赛牛处于偶数位置。

反转后，六头更赛牛处于偶数位置。不可能使得超过六头更赛牛处于偶数位置。

这道题和[翻硬币](https://www.acwing.com/activity/content/19/)类似，但有不同的地方，这道题是反转整个前缀而不是俩俩的翻。

将偶数长的前缀反转，可以将序列分组，

![image-20230616135525355](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230616135525355.png)

所以每次翻转的时候都是组与组之间反转，并且不会破坏相邻关系，只不过顺序可能会对调，比如反转 34 会变成 GH，但一定相邻。

分析不同组合: GG, HH, GH, HG 每次反转 GG 和 HH，都是不变的，可以忽略这两组的变化，GH <=> HG。所以可以直接令0代表HG,1代表GH，例如以上序列就可以表示为 001 ，目的是要把更多的G放在偶数位置上，即要更多的0。

例如 转换后的序列 0100101 要使得序列全为0

第一次反转索引 0 的组合，得到 1100101

第二次反转索引 1 之前的组合，得到 0000101

第三次反转索引 3 之前的组合，得到 1111101

第四次反转索引 4 之前的组合，得到 0000001

第五次反转索引 5 之前的组合，得到 1111111

最后反整个组合，得到0000000

可以看出 只要遇到不一样的位置，就需要反转一次，如果反转其他地方，那么这个位置还是不一样的，并且最后一个数是1的话，需要额外反转一次。

#### java

```java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int length = Integer.parseInt(sc.nextLine());
        String s = sc.nextLine();
        int count = 0;
        ArrayList<Integer> list = new ArrayList<>();
        for (int i = 0 ; i < length; i += 2){
            char p1 = s.charAt(i);
            char p2 = s.charAt(i+1);
            if ( (p1 == 'G') && (p2 == 'H')){
                list.add(1);
            }else if((p1 == 'H') && (p2 =='G')){
                list.add(0);
            }
        }
        int temp = list.get(0);
        for (int i = 1 ; i < list.size(); i++){
            if (temp != list.get(i)){
                count++;
            }
            temp = list.get(i);
        }
        if (list.get(list.size() - 1) == 1){
            count++;
        }
        System.out.println(count);
    }
}
```

