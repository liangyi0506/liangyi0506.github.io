# 几种常用的排序算法总结


# 总结

|              | 时间复杂度 |      空间复杂度      | 是否每次遍历都能确定一个数的位置 |               适用               |
| :----------: | :--------: | :------------------: | :------------------------------: | :------------------------------: |
| 直接插入排序 |   O(n^2)   |          1           |                否                |                                  |
|   希尔排序   |  O(nlogn)  |          1           |                否                |                                  |
|   冒泡排序   |   O(n^2)   |          1           |                是                |                                  |
| 简单选择排序 |   O(n^2)   |          1           |                是                |                                  |
|   快速排序   |  O(nlogn)  |          1           |                是                |                                  |
|   归并排序   |  O(nlogn)  |          n           |                                  |                                  |
|    堆排序    |  O(nlogn)  |          1           |                是                |                                  |
|   计数排序   |    O(n)    | k:k为n中最大最小值差 |           没有所谓循环           | 整数最大最小值差远远小于数组数量 |



## 1. 插入排序

#### 1.直接插入排序

> 基本思量：每一步将一个待排序都数据插入到已经排好序的序列之中，知道所以都元素查完为止

​		算法实现：

```java
public void straightSort(int[] arr){
    int n = arr.length;
    // 从1开始，因为arr[0]可以视作一个元素的有序数列，之后从1开始到序列视为无序数列
    for(int i = 1; i < n; i++){
        int temp = arr[i];
       	// 从已排序的序列的最后进行比较，找到合适的位置
        int j = i - 1;
        while(j >= 0 && arr[j] > temp){
            arr[j+1] = arr[j];
            j--;
        }
        arr[j+1] = temp;
    }
}
```

​	时间复杂度：O(n^2)

​	空间复杂度：T(1)

#### 2. 希尔排序

> 基本思想：将序列按照下标的一定增量分组，每个组中进行直接插入排序，然后增量减半，再再组内进行直接插入排序，。。。，直到增量减小为1时排序完成

​	算法实现:

```java
public void shellSort(int[] arr){
    int n = arr.length;
    for(int gap = n / 2; gap >= 1; gap/=2){
        for(int i = 0; i < gap; i++){
            // 进行插入排序，每个元素的下一个元素都是i+gap
            for(int j = i + gap; j < n; j+=gap){
                int temp = arr[j];
                int k = j-gap;
                while(k>=0 && arr[k] > temp){
                    arr[k+gap] = arr[k];
                    k-=gap;
                }
                
                arr[k+gap]= temp;
            }
        }
    }
}
```

​	时间复杂度：O(n*log2n)

​	空间复杂度：1

## 2.冒泡排序

#### 1.冒泡排序

> 基本思想：一种交换排序，一次只能将一个位置上的数字归位，第一次将[0, .... n-1]作为未排序数组，从0开始，如果num[i] > num[i+1], 那么交换两个数组，直到交换num[n-2]和num[n-1], 确定了num[n-1]的数子为[0,....,n-1]上最大的数字，之后确定[0, ...., n-2]上的最大数字交换至num[n-2],直到[0,1]

算法实现

```java
public void bubbleSort(int[] arr){
    int n = arr.length;
    for(int i = n-1; i >= 1; i--){
        for(int j = 0; j < i; j++){
            if(arr[j] > arr[j+1]){
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp; 
            }
        }
    }
}
```

​	时间复杂度：O(n^2)

​	空间复杂的：O(1)

#### 2.简单选择排序



> 基本思想：是一种冒泡排序，但是每次只交换为排序数组中最大的那个数和末尾的数，而不是发现顺序不对就交换，这也是思路最直接到排序方法

算法实现

```java
public void selectSort(int[] arr){
    int n = arr.length;
    for(int i = n-1; i <= 1; i--){
        int max = arr[i];
        int max_i = i;
        for(int j = 0; j < i; j++){
            if(arr[j] > max){
                max = arr[j];
                max_i = j;
            }
        }
        arr[max_i] = arr[i];
        arr[i] = max;
    }
}
```

​	时间复杂度：O(n^2)

​	空间复杂度：O(1)

## 3.分治法排序
#### 1.快速排序
> 基本思想：从待排序序列中任选一个元素，作为中间元素，作为pivot，将所有比pivot小的元素移动到它的左边，比pivot大的元素放在右边，pivot左右两边的子序列，重复前面的步骤，知道子序列不可再分

算法实现

```java
public void quickSort(int[] arr, int begin, int end){
    if(begin >=  end){
        return;
    }
    int mid = sort(arr, begin, end);
    quickSort(arr, begin, mid-1);
    quickSort(arr, mid + 1, end);
    
}

private void sort(int[] arr, int begin, int end){
    int p = begin;
    int q = end;
    int pivot = arr[p];
    while(p < q){
        // 从右往左找到第一个不大于pivot的数
        while(p < q && arr[q] > pivot){
            q--;
        }
        if(p < q){
            arr[p] = arr[q];
            p++;
        }
        // 从左往右找到第一个不小于pivot的数
        while(p < q && arr[p] < pivot){
            p++;
        }
        if(p < q){
            arr[q] = arr[p];
            q--;
        }
    }
    
    arr[p] = pivot;
    return p;
}
```

​	时间复杂度：O(nlogn)

​	空间复杂度：T(1)

## 4.堆排序

> 基本思想:构建一个堆，保证堆顶的数字是最大值（最小值）

算法实现

```java
public class HeapSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        int len = arr.length;

        buildMaxHeap(arr, len);

        for (int i = len - 1; i > 0; i--) {
            swap(arr, 0, i);
            len--;
            heapify(arr, 0, len);
        }
        return arr;
    }

    private void buildMaxHeap(int[] arr, int len) {
        for (int i = (int) Math.floor(len / 2); i >= 0; i--) {
            heapify(arr, i, len);
        }
    }

    private void heapify(int[] arr, int i, int len) {
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        int largest = i;

        if (left < len && arr[left] > arr[largest]) {
            largest = left;
        }

        if (right < len && arr[right] > arr[largest]) {
            largest = right;
        }

        if (largest != i) {
            swap(arr, i, largest);
            heapify(arr, largest, len);
        }
    }

    private void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

}
```

​	时间复杂度：O(nlogn)

​	空间复杂度：T(1)

## 5.计数排序

> 算法思路：统计整数序列中，每个数字出现的次数
>
> 步骤：1. 计算整数序列中的最大最小值max和min
>
> ​			2.新建序列 int[] counts = new int[max - min + 1]，统计每个数字出现的次数
>
> ​			3.读取counts重新排序
>
> 适用于：1. 整数序列 2. max - min  + 1<< n(序列长度)

算法实现

```java
public void sort(int[] arr){
    int min = Integer.MAX_VALUE;
    int max = Integer.MIN_VALUE;
    for(int num : arr){
        min = Math.min(num, min);
        max = Math.max(num, max);
    }
    
    int[] counts = new int[max - min + 1];
    int index = 0
    for(int i = 0; i < counts.length; i++){
        while(counts[i] > 0){
            arr[index++] = min + i;
            counts[i]--;
        }
    }
}
```

​	时间复杂度：O(n)

​	空间复杂度：O(k) k = max - min + 1



