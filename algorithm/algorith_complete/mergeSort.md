#### 归并排序

##### 算法思想

归并排序的算法思想和快排一样都是分治的思想，将长度为n的数组每次分一半，直到不能再分为止，然后使用一个新的数组来作为中转数组，将分成小块的数组以中间索引值为界限，比较值的大小，将值较小的先放进中转数组中，指针后移，将下一个数和上一次较大的数继续比较，重复此过程，直到到达中间索引，之后把剩下的数组中的数全部挪到中转数组中，最终把中转数组的值赋给原数组。

##### 图片演示

![img](https://img-blog.csdnimg.cn/20200209191150963.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RpZGxl,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20200209185525881.gif)

##### 代码实现

java

```java
public static void mergeSort(int[] arr,int left,int right,int[] temp){
        if (left < right){
            int mid = (left+right) / 2;
            mergeSort(arr,left,mid,temp);
            mergeSort(arr,mid+1,right,temp);
            merge(arr,left,right,mid,temp);
        }
    }
    //合并的方法

    /**
     *
     * @param arr
     * @param left 左边有序序列的初始索引
     * @param right 右边索引
     * @param mid 中间索引
     * @param temp 做中转的数组
     */
    public static void merge(int[] arr,int left,int right,int mid,int[] temp){
        int i = left;
        int j = mid+1;
        int t = 0;
        //2 4 8 9 6 5 4
        //0 1 2 3 4 5 6
        //1
        while (i <= mid && j <= right){
            if (arr[i] >= arr[j]){
                temp[t] = arr[j];
                j += 1;
                t += 1;
            }else {
                temp[t] = arr[i];
                i += 1;
                t += 1;
            }
        }
        //2
        while (i <= mid){
            temp[t] = arr[i];
            i += 1;
            t += 1;
        }
        while (j <= right){
            temp[t] = arr[j];
            j += 1;
            t += 1;
        }
        //3
        t = 0;
        int tempLeft = left;
        while (tempLeft <= right){
            arr[tempLeft] = temp[t];
            t += 1;
            tempLeft += 1;
        }
    }
```

python

```python

def mergeSort(nums,left,right,temp):
    if left < right:
        mid = (left + right) // 2
        #递归处理左子列表
        mergeSort(nums,left,mid,temp)
        #递归处理右子列表
        mergeSort(nums,mid+1,right,temp)
        #合并
        merge(nums,left,right,mid,temp)

def merge(nums,left,right,mid,temp):
    #用来操作数组的索引
    l = left
    r = mid+1
    t = 0
    #依次比较，将值小的先放进去
    while l <= mid and r <= right:
        if nums[l] < nums[r]:
            temp[t] = nums[l]
            l += 1
            t += 1
        else:
            temp[t] = nums[r]
            r += 1
            t += 1
    #将剩余数组挪进去
    while l <= mid:
        temp[t] = nums[l]
        l += 1
        t += 1
    while r <= right:
        temp[t] = nums[r]
        t += 1
        r += 1
    #将中转数组的值赋值给原数组
    t = 0
    tempLeft = left
    while tempLeft <= right:
        nums[tempLeft] = temp[t]
        t += 1
        tempLeft += 1


nums = [8,4,5,7,1,3,6,2]
temp = [_ for _ in range(len(nums))]
mergeSort(nums,0,len(nums)-1,temp)
print(nums)


```

##### 算法分析

归并排序使用的是分治的算法思想，将一个规模为n的问题分成两个规模为n/2的子问题，由下图的递归树可得最终分成了n个时间花费为c的子问题，此二叉树的深度为logn，所以最终的算法复杂度是O(nlogn)

![image-20230607221004942](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230607221004942.png)