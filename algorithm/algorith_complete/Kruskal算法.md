#### Kruskal算法

克鲁斯卡尔算法，是用来求加权连通图的最小生成树的算法。

![image-20230610154401892](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230610154401892.png)

#### 基本思想

按照权值从小到大的顺序，选择n-1条边，并保证这n-1条边不构成回路

#### 具体做法

首先构造一个只含n个顶点的森林，然后依权值从小到大从连通网中选择边加入到森林中，并使森林中不产生回路，直至森林变成一棵树为止

要点：

1.对边依据权值进行排序

2.将边加入森林，判断是否产生回路

解决方法：

1.选择任意一种排序方法排序

2.处理方式是：记录顶点在“最小生成树”中的终点，顶点的终点是“在最小生成树中与它连通的最大顶点”。然后每次需要将一条边添加到最小生成树中，判断该边的两个顶点的终点是否重合，重合的话则会构成回路。也就是说，要加入的边的两个顶点不能都指向同一个终点，否则将构成回路。

关于终点的说明：

就是将所有顶点按照从小到大的顺序排列好之后；某个顶点的终点就是“与他连通的最大顶点”。

过程：

![image-20230610180929093](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230610180929093.png)

![image-20230610180947452](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230610180947452.png)

用edges[]数组来保存各个边，并进行排序。在进行kruskal算法中，由于已经对边排好序，依次枚举出的边就是剩下的边中最小的。

有12条边，所以有12次循环。第1次循环中选择<E,F> = 2这条边，用getEnd()方法分别获取E点和F点的终点，由于第一次加入，这两点的终点就是他们本身，且终点不同，所以可以加入最小生成树中，终点相同时，例如第3步之后，开始第4次循环时，会选择<C,E> = 5这条边，继续用getEnd()求出C点、E点的终点，由第3步图中能看出，C、E的终点都是F点(按字母顺序),所以不符合要求，即加入这条边后会构成环，所以继续下一循环，重复此过程。



####  java

```java
package Kruskal;

import java.util.Arrays;

/**
 * @author Yan
 * @create 2023-06-10 11:31
 **/
public class kruskalCase {
    //边的个数
    private int edgeNum;
    //顶点数组
    private char[] vertexs;
    //邻接矩阵
    private int[][] matrix;
    private static final int INF = Integer.MAX_VALUE;

    public static void main(String[] args) {
        char[] vertexs = {'A','B','C','D','E','F','G'};
        int[][] matrix = {
                {0,12,INF,INF,INF,16,14},
                {12,0,10,INF,INF,7,INF},
                {INF,10,0,3,5,6,INF},
                {INF,INF,3,0,4,INF,INF},
                {INF,INF,5,4,0,2,8},
                {16,7,6,INF,2,0,9},
                {14,INF,INF,INF,8,9,0}
        };
        kruskalCase kruskalCase = new kruskalCase(vertexs, matrix);
//        kruskalCase.print();
//        EData[] edges = kruskalCase.getEdges();
//        System.out.println(Arrays.toString(edges));
//        System.out.println("排序后--------------");
//        kruskalCase.sortEdges(edges, 0,edges.length-1);
//        System.out.println(Arrays.toString(edges));
        kruskalCase.kruskal();
    }

    public kruskalCase(char[] vertexs, int[][] matrix){
        //初始化顶点数和边的个数
        int vlen = vertexs.length;
        this.vertexs = new char[vlen];
        System.arraycopy(vertexs, 0, this.vertexs, 0, vlen);
        this.matrix = new int[vlen][vlen];
        for(int i = 0 ; i < vlen;i++){
            for(int j = 0 ; j < vlen;j++){
                this.matrix[i][j] = matrix[i][j];
            }
        }
        //统计边
        for(int i = 0 ; i < vlen;i++){
            for(int j = i+1; j < vlen; j++){
                if (this.matrix[i][j] != INF){
                    edgeNum++;
                }
            }
        }

    }
    public void kruskal(){
        //表示最后结果数组的索引
        int index = 0;
        //保存已有最小生成树 中每一个顶点在最小生成树中的终点
        int[] ends = new int[edgeNum];
        //创建结果数组，保存最后的最小生成树
        EData[] results = new EData[edgeNum];
        EData[] edges = getEdges();
        sortEdges(edges,0,edgeNum-1);
        //遍历edges数组，将边添加到最小生成树中，判断是准备加入的边是否构成回路，如果没有，则加入，否则不能加入
        for (int i = 0; i < edgeNum; i++) {
            int p1 = getPosition(edges[i].start);
            int p2 = getPosition(edges[i].end);
            //获取p1这个顶点在已有最小生成树的终点
            int m = getEnd(ends,p1);
            //获取p2这个顶点在已有最小生成树的终点
            int n = getEnd(ends,p2);
            //是否构成回路
            if (m != n){//无回路
                ends[m] = n;
                results[index++] = edges[i];
            }
        }
        //最小生成树为
        for (int i = 0 ; i < index;i++){
            System.out.println(results[i]);
        }
    }

    public void print(){
        for(int i = 0 ; i < vertexs.length;i++){
            for (int j = 0 ; j < vertexs.length;j++){
                System.out.printf("%d\t",matrix[i][j]);
            }
            System.out.println();
        }
    }
    private void sortEdges(EData[] edges,int left,int right){
        int pivot = edges[(left + right) / 2].weight;
        int l = left;
        int r = right;
        EData temp;
        while (l < r){
            while(edges[l].weight < pivot){
                l++;
            }
            while(edges[r].weight > pivot){
                r--;
            }
            if (l >= r){
                break;
            }
            temp = edges[l];
            edges[l] = edges[r];
            edges[r] = temp;
            if (edges[l].weight == pivot){
                r--;
            }
            if (edges[r].weight == pivot){
                l++;
            }
        }
        if (l == r){
            l++;
            r--;
        }
        if (l < right){
            sortEdges(edges,l,right);
        }
        if (r > left){
            sortEdges(edges,left,r);
        }
    }

    /**
     *
     * @param ch 顶点的值 ‘A’ 'B'
     * @return 返回对应的下标
     */
    private int getPosition(char ch){
        for(int i = 0; i < vertexs.length; i++){
            if (vertexs[i] == ch){
                return i;
            }
        }
        return -1;
    }

    /**
     * 获取图中的边，后面需要遍历该数组
     * @return
     */
    private EData[] getEdges(){
        int index = 0;
        EData[] edges = new EData[edgeNum];
        for (int i = 0; i < vertexs.length; i++) {
            for (int j = i+1; j < vertexs.length; j++) {
                if (matrix[i][j] != INF){
                    edges[index++] = new EData(vertexs[i],vertexs[j],matrix[i][j]);
                }
            }
        }
        return edges;
    }

    /**
     * 获取下标为i的顶点的终点，用于后面判断两个顶点的终点是否相同
     * @param ends:记录了各个顶点对应的终点是那个，是在遍历过程中逐步形成的
     * @param i ： 表示传入的顶点对应的下标
     * @return 返回的就是下标为i的这个顶点对应的终点的下标
     */
    private int getEnd(int[] ends, int i){
        while (ends[i] != 0){
            i = ends[i];
        }
        return i;
    }

}

class EData{
    char start;
    char end;
    int weight;

    public EData(char start , char end,int weight){
        this.start = start;
        this.end = end;
        this.weight = weight;
    }

    @Override
    public String   toString() {
        return "EData{" +
                "<" + start +
                "," + end +
                ">,=" + weight +
                '}';
    }

}
```

#### python

```python
import sys
from typing import List

INF = sys.maxsize

class EData:
    def __init__(self, start, end, weight):
        self.start = start
        self.end = end
        self.weight = weight

    def getWeight(self):
        return self.weight

    def printData(self):
        print("EData{ %s => %s } = %s" % (self.start, self.end, self.weight))
        
class kruskalCase:
    edgesNum = 0
    def __init__(self, vertex, matrix):
        # 初始化
        n = len(vertex)
        self.vertex = [_ for _ in range(n)]
        self.matrix = [[_ for _ in range(n)] for _ in range(n)]
        for i in range(n):
            self.vertex[i] = vertex[i]
        for j in range(n):
            for k in range(n):
                self.matrix[j][k] = matrix[j][k]
        # 记录边
        for c in range(n):
            for m in range(c + 1, n):
                if self.matrix[c][m] != INF:
                    self.edgesNum += 1

    def kruskal(self):
        ends = [0 for _ in range(self.edgesNum)]
        edges = self.getEdges()
        temp = [_ for _ in range(self.edgesNum)]
        self.sortEdges(edges,0, self.edgesNum - 1, temp)
        results: List[EData] = [_ for _ in range(self.edgesNum)]
        index = 0
        for i in range(self.edgesNum):
            p1 = self.getPosition(edges[i].start)
            p2 = self.getPosition(edges[i].end)
            m = self.getEnd(p1, ends)
            n = self.getEnd(p2, ends)
            if m != n:
                ends[m] = n
                results[index] = edges[i]
                index += 1
        for k in range(index):
            results[k].printData()

    def getEdges(self):
        edges: List[EData] = []
        for i in range(len(self.vertex)):
            for j in range(i + 1, len(self.vertex)):
                if self.matrix[i][j] != INF:
                    edges.append(EData(self.vertex[i], self.vertex[j], self.matrix[i][j]))
        return edges

    def sortEdges(self, edges,left, right, temp):
        if left < right:
            mid = (left + right) // 2
            self.sortEdges(edges, left, mid, temp)
            self.sortEdges(edges,mid + 1, right, temp)
            self.merge(edges,left, right, mid, temp)

    def merge(self, edges,left, right, mid, temp):
        l = left
        r = mid + 1
        t = 0
        while l <= mid and r <= right:
            if edges[l].weight < edges[r].weight:
                temp[t] = edges[l]
                l += 1
                t += 1
            else:
                temp[t] = edges[r]
                r += 1
                t += 1
        while l <= mid:
            temp[t] = edges[l]
            t += 1
            l += 1
        while r <= right:
            temp[t] = edges[r]
            t += 1
            r += 1
        t = 0
        tempLeft = left
        while tempLeft <= right:
            edges[tempLeft] = temp[t]
            t += 1
            tempLeft += 1

    def getPosition(self, vertex) -> int:
        for i in range(len(self.vertex)):
            if self.vertex[i] == vertex:
                return i
        return -1

    def getEnd(self, vertex: int, ends):
        while ends[vertex] != 0:
            vertex = ends[vertex]
        return vertex

    def printWeights(self):
        for i in range(len(self.vertex)):
            for j in range(len(self.vertex)):
                print("%s\t" % self.matrix[i][j], end="")
            print()





vertex = ['A', 'B', 'C', 'D', 'E', 'F', 'G']
matrix = [
    [0, 12, INF, INF, INF, 16, 14],
    [12, 0, 10, INF, INF, 7, INF],
    [INF, 10, 0, 3, 5, 6, INF],
    [INF, INF, 3, 0, 4, INF, INF],
    [INF, INF, 5, 4, 0, 2, 8],
    [16, 7, 6, INF, 2, 0, 9],
    [14, INF, INF, INF, 8, 9, 0]
]
case = kruskalCase(vertex, matrix)
case.kruskal()
# edges = case.getEdges()
# for i in range(len(edges)):
#     print(type(edges[i]))
#     edges[i].printData()

```

