### Dijkstra

迪杰斯特拉算法是求图中某个顶点，到各个顶点的最短距离，体现了广度优先的思想，过程包括以下几个部分：

设置出发顶点为vi,顶点集合V{v1,v2,v3...vi},v到V中各顶点的距离记为集合dis{d1,d2,d3...di}

1.在集合dis中选择一个最小的值dj,dj即为vi到vj的最短距离，并记录他的下标,vj作为新的访问顶点

2.更新dis集合，规则为：出发顶点到访问顶点的距离与访问顶点到其他顶点vk的距离的和与出发顶点直接到vk的距离相比，保留最小值，同时记录前驱结点(出发顶点或者是访问顶点)

3.重复以上两步骤，直到遍历完所有结点

### 代码实现

#### java

```java
package dijikstraDemo;

import java.util.Arrays;
import java.util.jar.JarEntry;

/**
 * @author Yan
 * @create 2023-06-13 9:11
 **/
public class Dijkstra {
    static final int INF = 65535;
    public static void main(String[] args) {
        char[] vertex = {'A','B','C','D','E','F','G'};
        int[][] matrix = {
                {0,12,INF,INF,INF,16,14},
                {12,0,10,INF,INF,7,INF},
                {INF,10,0,3,5,6,INF},
                {INF,INF,3,0,4,INF,INF},
                {INF,INF,5,4,0,2,8},
                {16,7,6,INF,2,0,9},
                {14,INF,INF,INF,8,9,0}
        };
        Graph graph = new Graph(vertex, matrix);
//        graph.showGraph();
        graph.dsj(6);
        graph.showDijkstra();
    }

}

class Graph{
    //顶点数组
    private char[] vertex;
    private int[][] matrix;
    //已经访问的顶点的集合
    private VisitedVertex vv;
    public Graph(char[] vertex,int[][] matrix){
        this.matrix = matrix;
        this.vertex = vertex;
    }

    public void showGraph(){
        for(int[] link : matrix){
            System.out.println(Arrays.toString(link));
        }
    }

    public void dsj(int index){
        vv = new VisitedVertex(vertex.length, index);
        //更新index顶点到周围顶点的距离和前驱顶点
        update(index);
        for (int j = 1; j < vertex.length; j++){
            //选择并返回新的访问顶点
            index = vv.updateArr();
            //更新index顶点到周围顶点的距离和前驱顶点
            update(index);
        }
    }
    //更新index下标顶点到周围顶点的距离和周围顶点的前驱节点
    private void update(int index){
        int len = 0;
        for(int i = 0; i < matrix[index].length;i++){
            //出发顶点到index的距离 + 从index到i顶点
            len = vv.getDis(index) + matrix[index][i];
            //如果i顶点没有被访问过，并且len 小于出发顶点到j顶点的距离，就需要更新
            if (!vv.in(i) && len < vv.getDis(i)){
                vv.updatePre(i,index);
                vv.updateDis(i,len);
            }
        }
    }
    public void showDijkstra(){
        vv.show();
    }

}

class VisitedVertex{
    public int[] already_arr;
    public int[] pre_visited;
        public int[] dis;

    /**
     *
     * @param length:顶点的个数
     * @param index：出发顶点对应的下标，比如G顶点，下标就是6
     */
    public VisitedVertex(int length, int index){
        this.already_arr = new int[length];
        this.pre_visited = new int[length];
        this.dis = new int[length];
        Arrays.fill(dis,Dijkstra.INF);
        this.dis[index] = 0;
        this.already_arr[index] = 1;
    }
    //判断index顶点是否被访问过
    public boolean in(int index){
        return already_arr[index] == 1;
    }
    //更新出发顶点到index顶点的距离
    public void updateDis(int index, int len){
        dis[index] = len;
    }
    //更新顶点前驱为index的结点
    public void updatePre(int index, int pre){
        pre_visited[index] = pre;
    }
    //返回出发顶点到index顶点的距离
    public int getDis(int index){
        return dis[index];
    }

    public int updateArr(){
        int min = Dijkstra.INF, index = 0;
        for(int i = 0 ; i < already_arr.length;i++){
            if (already_arr[i] == 0 && dis[i] < min){
                min = dis[i];
                index = i;
            }
        }
        //更新index顶点
        already_arr[index] = 1;
        return index;
    }

    public void show(){
        System.out.println("==============");
        for (int i : already_arr){
            System.out.print(i + " ");
        }
        System.out.println();
        for (int i : pre_visited){
            System.out.print(i + " ");
        }
        System.out.println();
        for (int i : dis){
            System.out.print(i + " ");
        }
        System.out.println();
        char[] vertex = {'A','B','C','D','E','F','G'};
        int count = 0;
        for(int i : dis){
            if (i != Dijkstra.INF){
                System.out.print(vertex[count] + "("+i+")");
            }else {
                System.out.println("N ");
            }
            count ++;
        }
        System.out.println();
    }
}
```

#### python

```python
import sys

INF = sys.maxsize

class graph:

    def __init__(self,vertex,matrix):
        self.vv = None
        self.vertex = vertex
        self.matrix = matrix
        self.len = len(matrix[0])

    def showGraph(self):
        for i in range(self.len):
            for j in range(self.len):
                print(self.matrix[i][j])
        print()

    def dsj(self,index):
        self.vv = VisitedVertex(self.len,index)
        self.update(index)
        for i in range(1,self.len):
            index = self.vv.updateArr()
            self.update(index)
        self.showDijkstra()

    def update(self,index):
        for i in range(len(self.matrix[index])):
            long = self.vv.getDis(index) + self.matrix[index][i]
            if not self.vv.isVisited(i) and long < self.vv.getDis(i):
                self.vv.updateDis(i,long)
                self.vv.updatePre(i,index)

    def showDijkstra(self):
        self.vv.show()

class VisitedVertex:
    #pre_visited dis already_arr
    def __init__(self,length,index):
        self.already_arr = [0 for _ in range(length)]
        self.preVisited = [0 for _ in range(length)]
        self.dis = [0 for _ in range(length)]
        self.fill()
        self.already_arr[index] = 1
        self.dis[index] = 0

    def fill(self):
        for i in range(len(self.dis)):
            self.dis[i] = INF

    def isVisited(self,index):
        return self.already_arr[index] == 1

    def updateDis(self,index,len):
        self.dis[index] = len

    def updatePre(self,index,pre):
        self.preVisited[index] = pre

    def getDis(self,index):
        return self.dis[index]

    def updateArr(self):
        min = INF
        index = 0
        for i in range(len(self.already_arr)):
            if self.already_arr[i] == 0 and self.dis[i] < min:
                min = self.dis[i]
                index = i
        self.already_arr[index] = 1
        return index

    def show(self):
        print("======================")
        for i in range(len(self.already_arr)):
            print("%s " % self.already_arr[i],end="")
        print()
        for i in range(len(self.already_arr)):
            print("%s " % self.dis[i],end="")
        print()
        for i in range(len(self.already_arr)):
            print("%s " % self.preVisited[i],end="")
        print()
        self.vertexs = ['A','B','C','D','E','F','G']
        for i in range(len(self.already_arr)):
            if self.dis[i] != INF:
                print("%s(%s) " % (self.vertexs[i],self.dis[i]),end="")
            else:
                print("N",end="")


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
gra = graph(vertex,matrix)
gra.dsj(6)
```

