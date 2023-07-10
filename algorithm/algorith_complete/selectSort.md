#### 选择排序

##### 基本思想

找出列表元素中最小的数，记录下来，如果该数的索引不是初始位置，那就将该最小的数与初始位置的数替换，重复此过程。

![在这里插入图片描述](https://img-blog.csdnimg.cn/321fdce1d86546c496be8f8868ef097f.gif)

##### 代码实现

java

```java
public static void selectSort(int[] arr){
        for (int i = 0; i < arr.length - 1; i++) {
            int minIndex = i;
            int min = arr[i];
            for (int j = i + 1; j < arr.length; j++) {
                if (min > arr[j]){
                    min = arr[j];
                    minIndex = j;
                }
            }
            if (minIndex != i){
                arr[minIndex] = arr[i];
                arr[i] = min;
            }
            System.out.println(Arrays.toString(arr));
        }

    }
```

python

```python
#7 9 8 3 1 2
def selectSort(nums):
    n = len(nums)
    #因为最后一个数肯定是最大的，所以只需要循环n-1次
    for i in range(n-1):
        #假设最小值就是起始值
        minIndex = i
        minVal = nums[i]
        j = i + 1
        #遍历从索引位置为i+1到末尾的整个数组找到最小值并保存
        while j < n:
            if minVal > nums[j]:
                minVal = nums[j]
                minIndex = j
            j += 1
        #如果有其他的最小值则发生交换
        if minIndex != i:
            nums[minIndex] = nums[i]
            nums[i] = minVal
        print(nums)


selectSort([7,9,8,1,3,2])


```

##### 算法分析

选择排序的算法为O(n²).内层循环每次比较n-i次，(n-1)(n-2)(n-3).....1,T=[n*n-1]/2,根据计算方法保存最高次n².