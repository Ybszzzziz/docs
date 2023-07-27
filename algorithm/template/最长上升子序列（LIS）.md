### 最长上升子序列（LIS）

[896. 最长上升子序列 II - AcWing题库](https://www.acwing.com/problem/content/898/)

![image-20230722220019421](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230722220019421.png)

观察以3和1开始的上升子序列，可以接在3后面的子序列，就一定可以接在1后面，所以3就没有必要存下来了，将所有上升子序列按长度分类，长度为1的自序列只需要存最小的，以此类推，并且整个结尾数值应该是单调递增的

![image-20230722220719758](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230722220719758.png)

```java
import java.io.*;
import java.util.*;
public class Main{
    static int N = 100010;
    static int[] a = new int[N];
    //存的是不同长度的单调子序列里面结尾最小值
    static int[] q = new int[N];
    public static void main(String[] args) throws IOException{
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        for (int i = 0 ; i < n ; i ++) a[i] = sc.nextInt();
        //最长子序列长度
        int len = 0;
        q[0] = -2000000000;
        //用二分查找，找出小于a[i]的最大值
        for (int i = 0 ; i < n ; i ++){
            int l = 0, r = len;
            while (l < r){
                int mid = l + r + 1>> 1;
                if (q[mid] < a[i]) l = mid;
                else r = mid - 1;
            }
            //                a[i] 可以接在r的后面
            len = Math.max(len, r + 1);
            // r + 1 位置上的元素一定时 >= a[i]的，因为是单调上升，将该位置上的数变小
            // 替换，将数变小可以使单调子序列的长度更长(贪心)
            q[r + 1] = a[i];
        }
        System.out.println(len);
    }
    
}

```

