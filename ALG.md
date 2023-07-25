# 排序算法

## 选择排序

C++版本

```c++
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        int minIndex = i;
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        if (minIndex != i) {
            swap(arr[i], arr[minIndex]);
        }
    }
}
```

Java版本

```java
import java.util.Arrays;

public class SelectionSort {
    public static void selectionSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }

        int n = arr.length;
        // 外层循环控制每一趟排序的起始位置
        for (int i = 0; i < n - 1; i++) {
            int minIndex = i; // 未排序部分的最小元素索引
            // 内层循环找到未排序部分的最小元素
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            // 将找到的最小元素与未排序部分的起始位置交换
            if (minIndex != i) {
                int temp = arr[i];
                arr[i] = arr[minIndex];
                arr[minIndex] = temp;
            }
        }
    }
}
```



## 冒泡排序

C++版本

```c++
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        for (int j = 0; j < n - i - 1; ++j) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
            }
        }
    }
}
```

Java版本

```java
import java.util.Arrays;

public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }

        int n = arr.length;
        // 外层循环控制每一趟排序的次数
        for (int i = 0; i < n - 1; i++) {
            // 内层循环将当前最大元素冒泡到末尾
            for (int j = 0; j < n - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    // 交换相邻的两个元素
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
    }
}
```



## 插入排序

C++版本

```c++
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            --j;
        }
        arr[j + 1] = key;
    }
}
```

Java版本

```java
import java.util.Arrays;

public class InsertionSort {

    public static void insertionSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }

        int n = arr.length;
        // 外层循环从第二个元素开始，表示未排序部分的起始位置
        for (int i = 1; i < n; i++) {
            int current = arr[i]; // 待插入的元素
            int j = i - 1; // 已排序部分的末尾位置

            // 内层循环从已排序部分的末尾开始，找到插入位置
            while (j >= 0 && arr[j] > current) {
                arr[j + 1] = arr[j]; // 向后移动元素
                j--;
            }
            arr[j + 1] = current; // 插入元素到正确位置
        }
    }
}
```

##  希尔排序

C++版本

```c++
void shellSort(vector<int>& arr) {
    int n = arr.size();
    // 初始步长设置为数组长度的一半，不断缩小步长
    for (int gap = n / 2; gap > 0; gap /= 2) {
        // 对每个子序列进行插入排序
        for (int i = gap; i < n; ++i) {
            int temp = arr[i];
            int j = i;
            // 插入排序
            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }
            arr[j] = temp;
        }
    }
}
```

Java版本

```java
import java.util.Arrays;

public class ShellSort {

    public static void shellSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }

        int n = arr.length;
        // 初始步长为数组长度的一半
        int h = n / 2;

        while (h > 0) {
            // 对各个子序列进行插入排序
            for (int i = h; i < n; i++) {
                int j = i;
                int temp = arr[i];
                while (j >= h && arr[j - h] > temp) {
                    arr[j] = arr[j - h];
                    j -= h;
                }
                arr[j] = temp;
            }
            // 缩小步长
            h /= 2;
        }
    }
}
```

## 归并排序

C++版本

```c++
void merge(vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    vector<int> leftArr(n1);
    vector<int> rightArr(n2);
    
    for (int i = 0; i < n1; ++i) {
        leftArr[i] = arr[left + i];
    }
    for (int j = 0; j < n2; ++j) {
        rightArr[j] = arr[mid + 1 + j];
    }
    
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k] = leftArr[i];
            ++i;
        } else {
            arr[k] = rightArr[j];
            ++j;
        }
        ++k;
    }
    
    while (i < n1) {
        arr[k] = leftArr[i];
        ++i;
        ++k;
    }
    
    while (j < n2) {
        arr[k] = rightArr[j];
        ++j;
        ++k;
    }
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) {
        return;
    }
    
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}
```

Java版本

```java
import java.util.Arrays;

public class MergeSort {

    public static void mergeSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }

        int[] temp = new int[arr.length];
        mergeSort(arr, 0, arr.length - 1, temp);
    }

    private static void mergeSort(int[] arr, int left, int right, int[] temp) {
        if (left >= right) {
            return;
        }

        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid, temp); // 左半部分递归排序
        mergeSort(arr, mid + 1, right, temp); // 右半部分递归排序
        merge(arr, left, mid, right, temp); // 合并两个有序子数组
    }

    private static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        int i = left; // 左半部分的起始位置
        int j = mid + 1; // 右半部分的起始位置
        int k = left; // 合并后的数组的起始位置

        // 将两个有序子数组按顺序合并到temp数组中
        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                temp[k++] = arr[i++];
            } else {
                temp[k++] = arr[j++];
            }
        }

        // 处理剩余元素
        while (i <= mid) {
            temp[k++] = arr[i++];
        }

        while (j <= right) {
            temp[k++] = arr[j++];
        }

        // 将合并后的结果复制回原数组
        for (k = left; k <= right; k++) {
            arr[k] = temp[k];
        }
    }
}
```

## 堆排序

C++版本

```c++
void heapify(vector<int>& arr, int n, int i) {
    int largest = i; // 假设父节点最大
    int left = 2 * i + 1; // 左子节点的索引
    int right = 2 * i + 2; // 右子节点的索引

    // 如果左子节点比父节点大，更新最大值的索引
    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }

    // 如果右子节点比父节点大，更新最大值的索引
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }

    // 如果最大值不是父节点，则交换父节点和最大值
    if (largest != i) {
        swap(arr[i], arr[largest]);
        // 递归调整被交换的子树
        heapify(arr, n, largest);
    }
}

void heapSort(std::vector<int>& arr) {
    int n = arr.size();

    // 建立最大堆，从最后一个非叶子节点开始调整
    for (int i = n / 2 - 1; i >= 0; --i) {
        heapify(arr, n, i);
    }

    // 从堆顶开始取出元素，再调整堆
    for (int i = n - 1; i > 0; --i) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```

Java版本

```java
import java.util.Arrays;

public class HeapSort {

    public static void heapSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }

        // 构建最大堆
        buildMaxHeap(arr);

        // 堆排序
        int heapSize = arr.length;
        for (int i = arr.length - 1; i >= 1; i--) {
            swap(arr, 0, i); // 将堆顶元素与末尾元素交换
            heapSize--;
            heapify(arr, 0, heapSize); // 重新调整堆
        }
    }

    // 构建最大堆
    private static void buildMaxHeap(int[] arr) {
        int heapSize = arr.length;
        for (int i = arr.length / 2 - 1; i >= 0; i--) {
            heapify(arr, i, heapSize);
        }
    }

    // 调整堆
    private static void heapify(int[] arr, int i, int heapSize) {
        int leftChild = 2 * i + 1;
        int rightChild = 2 * i + 2;
        int largest = i;

        if (leftChild < heapSize && arr[leftChild] > arr[largest]) {
            largest = leftChild;
        }

        if (rightChild < heapSize && arr[rightChild] > arr[largest]) {
            largest = rightChild;
        }

        if (largest != i) {
            swap(arr, i, largest);
            heapify(arr, largest, heapSize);
        }
    }

    // 交换数组中的两个元素
    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

## 快速排序

C++版本

```c++
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high]; // 选择基准元素为最后一个元素
    int i = low - 1; // i 初始化为最小元素的索引

    for (int j = low; j < high; ++j) {
        // 如果当前元素小于或等于基准元素，将其交换到前面
        if (arr[j] <= pivot) {
            ++i;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]); // 将基准元素交换到正确的位置
    return i + 1; // 返回基准元素的索引
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        // pi 是划分索引，arr[pi] 已经在正确的位置
        int pi = partition(arr, low, high);

        // 递归地对划分的两个子数组进行快速排序
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

Java版本

```java
import java.util.Arrays;

public class QuickSort {

    public static void quickSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }
        quickSort(arr, 0, arr.length - 1);
    }

    private static void quickSort(int[] arr, int left, int right) {
        if (left >= right) {
            return;
        }

        // 选取基准元素（可以选择第一个元素、最后一个元素、中间元素等）
        int pivotIndex = partition(arr, left, right);

        // 对基准元素左边的子数组进行快速排序
        quickSort(arr, left, pivotIndex - 1);

        // 对基准元素右边的子数组进行快速排序
        quickSort(arr, pivotIndex + 1, right);
    }

    private static int partition(int[] arr, int left, int right) {
        // 选取第一个元素作为基准元素
        int pivot = arr[left];
		int nowLeft = left;
        while (left < right) {
            // 从右边开始找第一个小于基准元素的位置
            while (left < j && arr[right] >= pivot) {
                right--;
            }

            // 从左边开始找第一个大于基准元素的位置
            while (left < right && arr[left] <= pivot) {
                left++;
            }

            // 交换这两个位置的元素
            if (left < right) {
                swap(arr, left, right);
            }
        }

        // 将基准元素放入正确的位置
        swap(arr, left, nowLeft);

        // 返回基准元素的位置
        return left;
    }

    // 交换数组中的两个元素
    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

## 计数排序

C++ 版本

```c++
void countingSort(vector<int>& arr) {
    // 查找数组中的最大值，以确定计数数组的大小
    int max_element = arr[0];
    for (int num : arr) {
        if (num > max_element) {
            max_element = num;
        }
    }

    // 创建计数数组，并初始化为0
    vector<int> count_array(max_element + 1, 0);

    // 统计每个元素出现的次数
    for (int num : arr) {
        ++count_array[num];
    }

    // 更新计数数组，使得 count_array[i] 包含小于等于元素 i 的元素个数
    for (int i = 1; i <= max_element; ++i) {
        count_array[i] += count_array[i - 1];
    }

    // 创建临时数组存储排序结果
    vector<int> sorted_array(arr.size());

    // 遍历原始数组，将元素放入正确的位置
    for (int i = arr.size() - 1; i >= 0; --i) {
        sorted_array[count_array[arr[i]] - 1] = arr[i];
        --count_array[arr[i]];
    }

    // 将排序结果拷贝回原始数组
    arr = sorted_array;
}
```

Java版本

```java
import java.util.Arrays;

public class CountingSort {

    public static void countingSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }
        // 找出待排序数组中的最大值和最小值
        int maxVal = arr[0];
        int minVal = arr[0];
        for (int num : arr) {
            maxVal = Math.max(maxVal, num);
            minVal = Math.min(minVal, num);
        }

        // 确定计数数组的大小范围
        int range = maxVal - minVal + 1;

        // 创建计数数组，统计待排序数组中每个元素出现的次数
        int[] count = new int[range];
        for (int num : arr) {
            count[num - minVal]++;
        }

        // 计算每个元素在排序后数组中的位置
        for (int i = 1; i < range; i++) {
            count[i] += count[i - 1];
        }

        // 将待排序数组中的元素根据计数数组的统计信息放入正确的位置，完成排序
        int[] output = new int[arr.length];
        for (int i = arr.length - 1; i >= 0; i--) {
            int num = arr[i];
            int index = count[num - minVal] - 1;
            output[index] = num;
            count[num - minVal]--;
        }

        // 将排序结果复制回原数组
        System.arraycopy(output, 0, arr, 0, arr.length);
    }
}
```

## 桶排序

C++版本

```c++
void insertionSort(vector<int>& arr) {
    for (int i = 1; i < arr.size(); ++i) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            --j;
        }
        arr[j + 1] = key;
    }
}

void bucketSort(vector<int>& arr, int bucketSize = 5) {
    if (arr.empty()) {
        return;
    }

    int max_element = arr[0];
    int min_element = arr[0];
    for (int num : arr) {
        max_element = max(max_element, num);
        min_element = min(min_element, num);
    }

    int bucketCount = (max_element - min_element) / bucketSize + 1;
    vector<svector<int>> buckets(bucketCount);

    for (int num : arr) {
        int bucketIndex = (num - min_element) / bucketSize;
        buckets[bucketIndex].push_back(num);
    }

    arr.clear();
    for (vector<int>& bucket : buckets) {
        insertionSort(bucket);
        arr.insert(arr.end(), bucket.begin(), bucket.end());
    }
}
```

Java版本

```java
import java.util.ArrayList;
import java.util.Collections;

public class BucketSort {

    public static void bucketSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }

        // 找到最大值和最小值
        int maxVal = arr[0];
        int minVal = arr[0];
        for (int num : arr) {
            maxVal = Math.max(maxVal, num);
            minVal = Math.min(minVal, num);
        }

        // 确定桶的个数和每个桶的范围
        int bucketNum = arr.length;
        int bucketRange = (maxVal - minVal) / bucketNum + 1;

        // 创建桶
        ArrayList<ArrayList<Integer>> buckets = new ArrayList<>();
        for (int i = 0; i < bucketNum; i++) {
            buckets.add(new ArrayList<>());
        }

        // 将元素放入对应的桶中
        for (int num : arr) {
            int bucketIndex = (num - minVal) / bucketRange;
            buckets.get(bucketIndex).add(num);
        }

        // 对每个桶内的元素进行排序
        for (ArrayList<Integer> bucket : buckets) {
            Collections.sort(bucket);
        }

        // 合并各个桶的元素
        int index = 0;
        for (ArrayList<Integer> bucket : buckets) {
            for (int num : bucket) {
                arr[index++] = num;
            }
        }
    }
}

```

## 基数排序

C++版本

```c++
// 获取num的第digit位数字
int getDigit(int num, int digit) {
    return (num / static_cast<int>(pow(10, digit - 1))) % 10;
}

// 基数排序
void radixSort(vector<int>& arr) {
    int maxVal = *max_element(arr.begin(), arr.end());
    int digits = 0;
    while (maxVal > 0) {
        maxVal /= 10;
        digits++;
    }

    vector<int> count(10, 0);
    vector<int> output(arr.size());

    for (int i = 1; i <= digits; ++i) {
        fill(count.begin(), count.end(), 0);

        // 统计每个位的数字出现次数
        for (int j = 0; j < arr.size(); ++j) {
            int digit = getDigit(arr[j], i);
            count[digit]++;
        }

        // 将count[i]更新为包含小于等于i的元素个数
        for (int j = 1; j < count.size(); ++j) {
            count[j] += count[j - 1];
        }

        // 根据当前位的数字重新排序元素
        for (int j = arr.size() - 1; j >= 0; --j) {
            int digit = getDigit(arr[j], i);
            output[count[digit] - 1] = arr[j];
            count[digit]--;
        }
        // 将输出结果复制回原数组，准备下一轮排序
        copy(output.begin(), output.end(), arr.begin());
    }
}
```

Java版本

```java
import java.util.Arrays;
public class RadixSort {
    // 获取整数 num 的第 d 位数字
    private static int getDigit(int num, int d) {
        return (num / (int) Math.pow(10, d - 1)) % 10;
    }

    // 使用计数排序对数组按照某一位进行排序
    private static void countingSortByDigit(int[] arr, int digit) {
        int[] output = new int[arr.length];
        int[] count = new int[10];

        for (int num : arr) {
            int d = getDigit(num, digit);
            count[d]++;
        }

        for (int i = 1; i < 10; i++) {
            count[i] += count[i - 1];
        }

        for (int i = arr.length - 1; i >= 0; i--) {
            int num = arr[i];
            int d = getDigit(num, digit);
            output[count[d] - 1] = num;
            count[d]--;
        }
        System.arraycopy(output, 0, arr, 0, arr.length);
    }

    // 基数排序方法
    public static void radixSort(int[] arr) {
        if (arr == null || arr.length == 0) {
            return;
        }

        int maxNum = Arrays.stream(arr).max().getAsInt();
        int maxDigits = (int) Math.log10(maxNum) + 1;

        for (int digit = 1; digit <= maxDigits; digit++) {
            countingSortByDigit(arr, digit);
        }
    }
}
```







































































































