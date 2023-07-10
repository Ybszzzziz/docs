### AcWing 4908.饥饿的牛

[4908. 饥饿的牛 - AcWing题库](https://www.acwing.com/problem/content/description/4911/)

贝茜是一头饥饿的牛。

每天晚上，如果牛棚中还有干草的话，贝茜都会吃掉其中的一捆。

初始时，牛棚中没有干草。

为了让贝茜不被饿死，农夫约翰制定了 N个给贝茜送干草的计划。

其中第 i 个计划是在第 di 天的白天给贝茜送去 bi 捆干草。

这些计划互不冲突，保证 1≤d1<d2<…<dN≤T。

请你计算，贝茜在第 1∼T 天中有多少天有干草吃。

#### 输入格式

第一行包含两个整数 N 和 T。

接下来 N 行，每行包含两个整数 di,bi。

#### 输出格式

输出贝茜在第 1∼T天中有干草吃的天数。

#### 数据范围

1≤N≤10^5,
1≤T≤10^14,
1≤di≤10^14,
1≤bi≤10^9。

#### 输入样例1：

```
1 5
1 2
```

#### 输出样例1：

```
2
```

#### 样例1解释

两捆干草在第 1 天早上被送到了牛棚，所以贝茜第 1,2 天有干草吃。

#### 输入样例2：

```
2 5
1 2
5 10
```

#### 输出样例2：

```
3
```

#### 样例2解释

两捆干草在第 1 天早上被送到了牛棚，所以贝茜第 1,2 天有干草吃。

10 捆干草在第 5 天早上被送到了牛棚，所以贝茜第 5 天有干草吃。

#### 输入样例3：

```
2 5
1 10
5 10
```

#### 输出样例3：

```
5
```

10 捆干草在第 1 天早上被送到了牛棚，所以贝茜第 1∼5天都有干草吃。

数据范围到10^14，超出了int的范围(-2^31 - 2^31-1)，所以用long来存储。

画图方便理解：

![image-20230616131641181](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230616131641181.png)

数轴范围 1 ~ T，数轴上的点d1,d2...di,表示送草的日期，b1,b2...bi，表示送草的数量。cur 变量表示，在d2 - 1天的草的数量，cur 经过 d2 天后所拥有的草的数量是 cur + b2, 用 len 表示 d2 天到 d3 - 1天的天数，d2 天到 d3 - 1天吃掉的数量就是 days = min{cur + b2, len} ，cur 的下一个位置就是 cur + b2 - days.

下面张图更适合用代码实现

![image-20230616132526542](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230616132526542.png)

直接令 cur 之前的日期为 last，不用去考虑last，cur 为last之后一天所拥有的草的数量，中间区间 len = d3 - 1 - last , 区间内吃草的天数 days = min{cur, len}, 下一区间 cur = cur - days + b3 , last = d3 - 1 , 最后还需处理 last + 1到 T 的区间内吃草的天数，即 min{cur, T - last}.

#### java代码实现

```java
import java.io.*;
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        long cur = 0;
        long days = 0;
        long last = 0;
        long count = 0;
        String[] strings = new String[0];
        try {
            strings = bf.readLine().split(" ");
        } catch (IOException e) {
            e.printStackTrace();
        }
        long n = Long.parseLong(strings[0]);
        long t = Long.parseLong(strings[1]);
        for(int i = 1 ; i <= n; i++){
            String[] s1 = new String[0];
            try{
                s1 = bf.readLine().split(" ");
            }catch (IOException e){
                e.printStackTrace();
            }
            long d = Long.parseLong(s1[0]);
            long b = Long.parseLong(s1[1]);
            days = Math.min(cur, d - last - 1);
            cur = cur - days + b;
            count += days;
            last = d - Long.parseLong(String.valueOf(1));
        }
        count += Math.min(cur, t - last);
        System.out.println(count);
    }
}
```

