#### 冒泡排序

##### 算法步骤

冒泡排序是最基础的排序，其主要步骤是：比较相邻的元素，如果第一个比第二个大，就交换他们两个，也就是从第一对索引为0和1的数到索引为n-1到n的数进行交换操作，除最后一个数之外，对所有的元素重复以上的步骤，直到没有数字可以进行交换

##### 动图演示

![img](https://www.runoob.com/wp-content/uploads/2019/03/bubbleSort.gif)

java

```java
public class bubble {
    public static void main(String[] args) {
        boolean flag = false;
        int temp = 0;
        int[] arr = new int[]{2,3,4,1,5,9,0};
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length - i - 1; j++) {
                if (arr[j] > arr[j+1]){
                    temp = arr[j+1];
                    arr[j+1] = arr[j];
                    arr[j] = temp;
                    flag = true;
                }

            }
            System.out.println(Arrays.toString(arr));
            if (!flag){
                break;
            }else {
                flag = false;
            }
        }
    }
}

```

python

```python
# 7 9 8 3 1 2
def bubbleSort(nums: list):
    n = len(nums)
    for i in range(n):
        j = 0
        flag = False
        #因为每一次调整都把范围中最大数的放在了最后一位
        #所以下一次调整的循环就应该少一次
        while j < n - i - 1:
            if nums[j] > nums[j+1]:
                temp = nums[j+1]
                nums[j+1] = nums[j]
                nums[j] = temp
                flag = True
            j += 1
        #当没有发生交换时说明列表已经有序，所以就跳出循环
        if not flag:
            break
        print(nums)

bubbleSort([7,9,8,3,1,2])


```

##### 时间复杂度分析

在最坏的情况下，基本操作就是交换，此时内层循环每次都会交换，所以时间复杂度就是(n-1)(n-2)(n-3)......1等差数列的和n(n-1)/2 = O(n²)