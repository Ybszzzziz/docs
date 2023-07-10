### Trie树

#### 例题：Trie字符串统计

[835. Trie字符串统计 - AcWing题库](https://www.acwing.com/problem/content/837/)

![image-20230702201849172](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230702201849172.png)

```java
import java.io.*;
public class Main{
    static int N = 100010;
    static int[][] son = new int[N][26];
    static int[] cnt = new int[N];
    static int idx = 0;
    public static void main(String[] args) throws IOException{
        StreamTokenizer st = new StreamTokenizer(new BufferedReader(new InputStreamReader(System.in)));
        st.nextToken();int n = (int)st.nval;
        while (n-- > 0){
            st.nextToken();String str = st.sval;
            if ("I".equals(str)){
                st.nextToken();str = st.sval;
                insert(str.toCharArray());
            }else{
                st.nextToken();str = st.sval;
                int c = query(str.toCharArray());
                System.out.println(c);
            }
        }
        
    }
    
    public static void insert(char[] str){
        int p = 0;
        for (int i = 0 ; i < str.length; i++){
            //映射到 0-25
            int u = str[i] - 'a';
            if (son[p][u] == 0) son[p][u] = ++idx;
            p = son[p][u];
        }
        //遍历完整个字符后，在末尾记录数量
        cnt[p]++;
        
    }
    
    public static int query(char[] str){
        int p = 0;
        for (int i = 0; i < str.length; i++){
            int u = str[i] - 'a';
            //说明 无此字符串
            if (son[p][u] == 0) return 0;
            p = son[p][u];
        }
        //返回出现次数
        return cnt[p];
        
    }
    
}
```





[143. 最大异或对 - AcWing题库](https://www.acwing.com/problem/content/145/)![image-20230703164126131](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230703164126131.png)

```java
import java.io.*;
public class Main{
    static int N = 100010, M = 31 * N;
    static int[][] son = new int[M][2];
    static int[] p = new int[N];
    static int idx = 0;
    public static void main(String[] args) throws IOException{
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(bf.readLine());
        String[] str1 = bf.readLine().split(" ");
        int res = 0;
        for (int i = 0 ; i < n ; i++) p[i] = Integer.parseInt(str1[i]);
        for (int i = 0 ; i < n; i++){
            insert(p[i]);
            int val = query(p[i]);
            res = Math.max(res,p[i] ^ val);
        }
        System.out.println(res);
    }
    
    public static void insert(int x){
        int p = 0;
        for (int i = 30 ; i >= 0; i--){
            //取出最后一位
            int u = x >> i & 1;
            if (son[p][u] == 0) son[p][u] = ++idx;
            p = son[p][u];
        }
    }
    
    public static int query(int x){
        int p = 0, t = 0;
        for (int i = 30 ; i >= 0; i--){
            int u = x >> i & 1;
            if (son[p][1-u] != 0){
                t = (t << 1) + 1 - u;
                p = son[p][1-u];
            }else{
                t = (t << 1) + u;
                p = son[p][u];
            }
        }
        return t;
    }
    
}
```

