#### 快速排序

##### 算法思想

快速排序使用了分而治之(divide and conquer)的算法思想,在数组中定义一个中间值pivot，一般为头部、尾部或者中间索引位置，在每次的快排中，找出pivot左边第一个比pivot值大的数和pivot右边第一个比pivot值小的数，将这两数交换，之后递归的处理满足条件的左子数组和右子数组。

##### 代码实现

java

```java
 public static void quickSort(int[] arr,int left,int right){
        int l = left;
        int r = right;
        int pivot = arr[(left+right) / 2];
        int temp = 0;
        while (l < r){
            while (arr[l] < pivot){
                l += 1;
            }
            while (arr[r] > pivot){
                r -= 1;
            }
            if (l >= r){
                break;
            }
            temp = arr[l];
            arr[l] = arr[r];
            arr[r] = temp;
            //
            if (arr[l] == pivot){
                r -= 1;
            }
            if (arr[r] == pivot){
                l += 1;
            }
        }
        if(l == r){
            l += 1;
            r -= 1;
        }
        if (left < r){
            quickSort(arr,left,r);
        }
        if (right > l){
            quickSort(arr,l,right);
        }

    }
```

python

```python
def quickSort(nums,left,right):
    #假设pivot是中间值
    pivot = nums[(left + right) // 2]
    l = left
    r = right
    while l < r:
        # 直到找到pivot左边的数大于pivot的索引
        while nums[l] < pivot:
            l += 1
        # 找到右边小于pivot的值的索引
        while nums[r] > pivot:
            r -= 1
        if l >= r:
            break
        #交换 大于或等于pivot的值与小于或等于pivot的值交换
        temp = nums[l]
        nums[l] = nums[r]
        nums[r] = temp
        #如果交换过后pivot跑到了左边
        #说明 大于等于r索引上的数都大于等于pivot,此时r指针应该前移
        if nums[l] == pivot:
            r -= 1
        #同理
        if nums[r] == pivot:
            l += 1
    #错开
    if l == r:
        l += 1
        r -= 1
    #递归的对右子数组和左子数组排序
    if l < right:
        quickSort(nums,l,right)
    if r > left:
        quickSort(nums,left,r)
```

##### 算法复杂度分析

首先用一个简单的代码示范怎么计算logn

```python
i = 2
count = 0
while i <= 1024:
    i = i*2
    count += 1
print(count)
```

输出是10，也就是程序运行了10次，也就是lg1024 = 10

在快速排序的递归过程中每次将数组分成一半，此时为O(logn)的时间复杂度，在每次的递归中遍历每个数把比pivot小的放它左边，比pivot大的放它右边所需时间复杂度为O(n)，连在一起就是O(nlogn)