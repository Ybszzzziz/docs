### 最短Hamilton路径

[91. 最短Hamilton路径 - AcWing题库](https://www.acwing.com/problem/content/93/)

![image-20230723170418346](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230723170418346.png)

```java
import java.io.*;
import java.util.*;
public class Main{
    static int N = 20, M = 1 << N;
    static int[][] w = new int[N][N];
    static int[][] f = new int[M][N];
    public static void main(String[] args) throws IOException{
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        for (int i = 0 ; i < n ; i ++){
            for (int j = 0 ; j < n ; j ++) w[i][j] = sc.nextInt();
        }
        for (int i = 0 ; i < M ; i ++) Arrays.fill(f[i], 0x3f3f3f3f);
        // 初始化第一个点 从0到0 经过0这个点，距离为0
        f[1][0] = 0;
        // 枚举各个状态
        for (int i = 0 ; i < 1 << n ; i ++){
            // 枚举终点
            for (int j = 0 ; j < n ; j ++){
                // 如果第i状态包含第j点
                if ((i >> j & 1) == 1){
                    for (int k = 0 ; k < n ; k ++){
                        //如果第i状态删去第j点包含第k点
                        if ((i - (1 << j) >> k & 1) == 1){
                            f[i][j] = Math.min(f[i][j], f[i - (1 << j)][k] + w[k][j]);
                        }
                    }
                }
            }
        }
        // 从0走到n-1， 经过所有点，即每个点的位置用二进制表示都是1
        // 例如 n = 3 , 1 << 3 = 8 - 1 = 7 = 1 1 1 ;
        System.out.println(f[(1 << n) - 1][n - 1]);
        
    }
    
}
```

