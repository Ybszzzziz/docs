#### 插入排序

##### 算法思想

插入排序字面意思就是依靠插入来排序，在排序过程中分成已排序数组和未排序数组，内部循环在已排序数组中找到要插入数值的插入位置，然后进行插入操作，最后通过外部循环将未排序数组的尾部和排序数组的头部后移，不断循环此过程直到整个数组是已排序数组。

##### 动图演示

![动图](https://pic3.zhimg.com/v2-91b76e8e4dab9b0cad9a017d7dd431e2_b.webp)

##### 算法实现

###### java

```java
public static void insertSort(int[] nums){
        for(int i = 1; i < nums.length; i++){
            int insertVal = nums[i];
            int insertIndex = i - 1;
            while (insertIndex >= 0 && insertVal < nums[insertIndex]){
                nums[insertIndex+1] = nums[insertIndex];
                insertIndex--;
            }
            nums[insertIndex+1] = insertVal;
        }
    }
```

###### python

```python
def insertSort(nums):
    i = 1
    n = len(nums)
    #索引位置从1到n-1,包括最后一个数
    while i < n:
        #要插入的数值
        insertVal = nums[i]
        #初始要插入的位置
        insertIndex = i - 1
        #循环找到位置，数组不能越界，并且要插入的数比插入位置的元素小
        while insertIndex >= 0 and insertVal < nums[insertIndex]:
            #后移插入位置上的数
            nums[insertIndex+1] = nums[insertIndex]
            insertIndex -= 1
        #循环结束表示找到了插入位置,进行插入
        nums[insertIndex+1] = insertVal
        i += 1
```

##### 算法复杂度

最好情况就是全部有序，此时只需遍历一次，最好的时间复杂度为O(n)，最坏的情况下每次的外部循环中，内部循环要循环i次才能找到插入位置，也就是1 * 2 * 3 * 4 * ... * n-1，                               T=[{(1+n-1)n}/2]，O(n) = n².