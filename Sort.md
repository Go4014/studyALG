# 简要比较

![image-20240516155901072](images/image-20240516155901072.png)

- 均按从小到大排列
- k代表数值中的"数位"个数
- n代表数据规模
- m代表数据的最大值减最小值

# 数组排序

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

// 优化
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; ++j) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if(!swapped) {
            break;
        }
    }
}

// 再优化
void bubbleSort(vector<int>& arr) {
    int n = arr.size() - 1;
    while(n != 0) {
        int lastSwapIndex = 0;
        for (int j = 0; j < n; ++j) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                lastSwapIndex = j;
            }
        }
        n = lastSwapIndex;
    }
}
```

Java版本

```java
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

// 优化
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }

        int n = arr.length;
        // 外层循环控制每一趟排序的次数
        for (int i = 0; i < n - 1; i++) {
            boolean swapped = false;
            // 内层循环将当前最大元素冒泡到末尾
            for (int j = 0; j < n - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    // 交换相邻的两个元素
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            if(!swapped) {
                break;
            }
        }
    }
}

// 再优化
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        if (arr == null || arr.length <= 1) {
            return;
        }

        int n = arr.length - 1;
        // 外层循环控制每一趟排序的次数
        while(n != 0) {
            int lastSwapIndex = 0;
            // 内层循环将当前最大元素冒泡到末尾
            for (int j = 0; j < n; j++) {
                if (arr[j] > arr[j + 1]) {
                    // 交换相邻的两个元素
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    lastSwapIndex = j;
                }
            }
            n = lastSwapIndex;
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
class Solution {
public:
    void sortArray(vector<int>& nums) {
        int gap = nums.size() / 2;
        while(gap > 0) {
            for(int i = gap; i < nums.size(); i++) {
                int temp = nums[i];
                int j = i;
                while(j >= gap && nums[j - gap] > temp) {
                    nums[j] = nums[j - gap];
                    j -= gap;
                }
                nums[j] = temp;
            }
            gap /= 2;
        }
    }
};
```

Java版本

```java
public class ShellSort {

    public static void shellSort(int[] nums) {
        if (nums == null || nums.length <= 1) {
            return;
        }
        int gap = nums.length / 2;
        while(gap > 0) {
            for(int i = gap; i < nums.length; i++) {
                int temp = nums[i];
                int j = i;
                while(j >= gap && nums[j - gap] > temp) {
                    nums[j] = nums[j - gap];
                    j -= gap;
                }
                nums[j] = temp;
            }
            gap /= 2;
        }
    }
}
```

## 归并排序

C++版本

```c++
class Solution {
public:
    void sortArray(vector<int>& nums) {
        vector<int> temp(nums.size());
        mergeSort(nums, 0, nums.size() - 1, temp);
    }

    void mergeSort(vector<int>& nums, int left, int right, vector<int>& temp) {
        if(left >= right) {
            return;
        }
        int mid = (left + right) / 2; // (right - left) / 2 + left
        mergeSort(nums, left, mid, temp);
        mergeSort(nums, mid+1, right, temp);
        merge(nums, left, mid, right, temp);
    }

    void merge(vector<int>& nums, int left, int mid, int right, vector<int>& temp) {
        int i = left; // 左半部分的起始位置
        int j = mid + 1; // 右半部分的起始位置
        int index = left; // 合并后的数组的起始位置

        // 将两个有序子数组按顺序合并到temp数组中
        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                temp[index++] = arr[i++];
            } else {
                temp[index++] = arr[j++];
            }
        }

        // 处理剩余元素
        while (i <= mid) {
            temp[index++] = arr[i++];
        }
        while (j <= right) {
            temp[index++] = arr[j++];
        }

        // 将合并后的结果复制回原数组
        for (index = left; index <= right; index++) {
            arr[index] = temp[index];
        }
    }
};
```

Java版本

```java
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
        mergeSort(arr, mid+1, right, temp); // 右半部分递归排序
        merge(arr, left, mid, right, temp); // 合并两个有序子数组
    }

    private static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        int i = left; // 左半部分的起始位置
        int j = mid + 1; // 右半部分的起始位置
        int index = left; // 合并后的数组的起始位置

        // 将两个有序子数组按顺序合并到temp数组中
        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                temp[index++] = arr[i++];
            } else {
                temp[index++] = arr[j++];
            }
        }

        // 处理剩余元素
        while (i <= mid) {
            temp[index++] = arr[i++];
        }
        while (j <= right) {
            temp[index++] = arr[j++];
        }

        // 将合并后的结果复制回原数组
        for (index = left; index <= right; index++) {
            arr[index] = temp[index];
        }
    }
}
```

## 堆排序

C++版本

```c++
class Solution {
public:
    void sortArray(vector<int>& nums) {
        int len = nums.size();
        buildMaxHeap(nums);
        for(int i = 0; i < len; i++) {
            swap(nums[0], nums[len - i - 1]);
            heapify(nums, 0, len - i - 2);
        }
    }

    void buildMaxHeap(vector<int>& nums) {
        int len = nums.size();
        // [(len - 2) / 2 == (len / 2) - 1]
        for(int i = (len - 2) / 2; i >= 0; i--) { 
            heapify(nums, i, len - 1);
        }
    }

    void heapify(vector<int>& nums, int index, int end) {
        int left = index * 2 + 1;
        int right = left + 1;
        while(left <= end) {
            int maxIndex = index;
            if(nums[left] > nums[maxIndex]) {
                maxIndex = left;
            }
            if(right <= end && nums[right] > nums[maxIndex]) {
                maxIndex = right;
            }
            if(maxIndex == index) {
                break;
            }
            swap(nums[index], nums[maxIndex]);
            index = maxIndex;
            left = index * 2 + 1;
            right = left + 1;
        }
    }
};
```

Java版本

```java
public class HeapSort {
    public void heapSort(int[] nums) {
        int heapSize = nums.length;
        
        // 构建最大堆
        buildMaxHeap(nums);
        
        // 堆排序
        for (int i = 0; i <= nums.length; i++) {
            swap(nums, 0, heapSize - i - 1);
            heapify(nums, 0, heapSize - i - 2);
        }
    }

    // 构建最大堆
    public void buildMaxHeap(int[] arr) {
        int heapSize = nums.length;
        for (int i = (heapSize - 2) / 2; i >= 0; --i) { // (heapSize - 2) / 2 => 表示最后一个非叶子节点
            heapify(arr, i, heapSize - 1);
        } 
    }

    // 调整堆
    public void heapify(int[] arr, int index, int end) {
        int left = index * 2 + 1;
        int right = left + 1;
        
        while(left <= end) {
            int maxIndex = index;
            if(nums[left] > nums[maxIndex]) {
                maxIndex = left;
            }
            if(right <= end && nums[right] > nums[maxIndex]) {
                maxIndex = right;
            }
            if(maxIndex == index) {
                break;
            }
            swap(nums[index], nums[maxIndex]);
            index = maxIndex;
            left = index * 2 + 1;
            right = left + 1;
        }
    }

    // 交换数组中的两个元素
    public void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

## 快速排序

C++版本

```c++
class Solution {
public:
    void sortArray(vector<int>& nums) {
        quickSort(nums, 0, nums.size() - 1);
    }

    void quickSort(vector<int>& nums, int left, int right) {
        if(left < right) {
            int point = partion(nums,left,right);
            quickSort(nums, left, point-1);
            quickSort(nums, point+1, right);
        }
    }

    int partion(vector<int>& nums, int left, int right) {
        int index = rand() % (right - left + 1) + left;
        swap(nums[left], nums[index]);
        int key = nums[left];
        while(left < right) {
            while(left < right && nums[right] >= key) right--;
            if(left < right) nums[left] = nums[right];
            
            while(left < right && nums[left] <= key) left++;
            if(left < right) nums[right] = nums[left];
        }
        nums[left] = key;
        return left;
    }
};
```

Java版本

```java
class Solution {
    public void sortArray(int[] nums) {
        quicksort(nums, 0, nums.length - 1);
    }

    public void quicksort(int[] nums, int left, int right) {
        if (left < right) {
            int pos = partition(nums, left, right);
            quicksort(nums, left, pos - 1);
            quicksort(nums, pos + 1, right);
        }
    }

    public int partition(int[] nums, int left, int right) {
        // 随机选一个作为我们的主元
        int index = new Random().nextInt(right - left + 1) + left;
        swap(nums, left, index);
        
        int key = nums[left];
        while(left < right) {
            while(left < right && nums[right] >= key) right--;
            if(left < right) nums[left] = nums[right];
            
            while(left < right && nums[left] <= key) left++;
            if(left < right) nums[right] = nums[left];
        }
        nums[left] = key;
        return left;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

## 计数排序

C++ 版本

```c++
void countingSort(vector<int>& arr) {
    // 查找数组中的最大值/最小值，以确定计数数组的大小
    int max_element = arr[0];
    int min_element = arr[0];
    for (int num : arr) {
        if (num > max_element) {
            max_element = num;
        }
        if (num < min_element) {
            min_element = num;
        }
    }

    // 创建计数数组，并初始化为0
    vector<int> count_array(max_element - min_element + 1, 0);

    // 统计每个元素出现的次数
    for (int num : arr) {
        ++count_array[num - min_element];
    }

    // 更新计数数组，使得 count_array[i] 包含小于等于元素 i 的元素个数
    for (int i = 1; i <= max_element - min_element + 1; ++i) {
        count_array[i] += count_array[i - 1];
    }

    // 创建临时数组存储排序结果
    vector<int> sorted_array(arr.size());

    // 遍历原始数组，将元素放入正确的位置
    for (int i = arr.size() - 1; i >= 0; --i) {
        sorted_array[count_array[arr[i] - min_element] - 1] = arr[i];
        --count_array[arr[i] - min_element];
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
            output[count[arr[i] - minVal] - 1] = arr[i];
            count[arr[i] - minVal]--;
        }

        // 将排序结果复制回原数组
        System.arraycopy(output, 0, arr, 0, arr.length);
    }
}
```

## 桶排序

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
    vector<vector<int>> buckets(bucketCount);

    for (int num : arr) {
        int bucketIndex = (num - min_element) / bucketSize;
        buckets[bucketIndex].push_back(num);
    }

    arr.clear();
    for (vector<int>& bucket : buckets) {
        shellSort(bucket);
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



# 链表排序

## 冒泡排序

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* bubbleSort(ListNode* head) {
        ListNode* nodeI = head;
        ListNode* tail = nullptr;

        while (nodeI) {
            ListNode* nodeJ = head;
            while (nodeJ && nodeJ->next != tail) {
                if (nodeJ->val > nodeJ->next->val) {
                    // 交换两个节点的值
                    int temp = nodeJ->val;
                    nodeJ->val = nodeJ->next->val;
                    nodeJ->next->val = temp;
                }
                nodeJ = nodeJ->next;
            }
            // 尾指针向前移动 1 位，此时尾指针右侧为排好序的链表
            tail = nodeJ;
            nodeI = nodeI->next;
        }

        return head;
    }
    
    ListNode* sortList(ListNode* head) {
        return bubbleSort(head);
    }
};
```

Java版本

```java
public class Solution {
    public ListNode bubbleSort(ListNode head) {
        ListNode nodeI = head;
        ListNode tail = null;

        while (nodeI != null) {
            ListNode nodeJ = head;
            while (nodeJ != null && nodeJ.next != tail) {
                if (nodeJ.val > nodeJ.next.val) {
                    // 交换两个节点的值
                    int temp = nodeJ.val;
                    nodeJ.val = nodeJ.next.val;
                    nodeJ.next.val = temp;
                }
                nodeJ = nodeJ.next;
            }
            // 尾指针向前移动 1 位，此时尾指针右侧为排好序的链表
            tail = nodeJ;
            nodeI = nodeI.next;
        }

        return head;
    }
    
    public ListNode sortList(ListNode head) {
        return bubbleSort(head);
    }
}
```



## 选择排序

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* selectionSort(ListNode* head) {
        ListNode* nodeI = head;

        while (nodeI && nodeI->next) {
            ListNode* minNode = nodeI;
            ListNode* nodeJ = nodeI->next;

            while (nodeJ) {
                if (nodeJ->val < minNode->val) {
                    minNode = nodeJ;
                }
                nodeJ = nodeJ->next;
            }

            if (nodeI != minNode) {
                swap(nodeI->val, minNode->val);
            }

            nodeI = nodeI->next;
        }

        return head;
    }
    
    ListNode* sortList(ListNode* head) {
        return selectionSort(head);
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode selectionSort(ListNode head) {
        ListNode nodeI = head;

        while (nodeI != null && nodeI.next != null) {
            ListNode minNode = nodeI;
            ListNode nodeJ = nodeI.next;

            while (nodeJ != null) {
                if (nodeJ.val < minNode.val) {
                    minNode = nodeJ;
                }
                nodeJ = nodeJ.next;
            }

            if (nodeI != minNode) {
                int temp = nodeI.val;
                nodeI.val = minNode.val;
                minNode.val = temp;
            }

            nodeI = nodeI.next;
        }

        return head;
    }
    
    public ListNode sortList(ListNode head) {
        return selectionSort(head);
    }
}
```



## 插入排序

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* insertionSort(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }

        ListNode* dummyHead = new ListNode(-1);
        dummyHead->next = head;
        ListNode* sortedList = head;
        ListNode* cur = head->next;

        while (cur) {
            if (sortedList->val <= cur->val) {
                // 将 cur 插入到 sortedList 之后
                sortedList = sortedList->next;
            } else {
                ListNode* prev = dummyHead;
                while (prev->next->val <= cur->val) {
                    prev = prev->next;
                }
                // 将 cur 插入到链表中间
                sortedList->next = cur->next;
                cur->next = prev->next;
                prev->next = cur;
            }
            cur = sortedList->next;
        }

        return dummyHead->next;
    }
    
    ListNode* sortList(ListNode* head) {
        return insertionSort(head);
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode insertionSort(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }

        ListNode dummyHead = new ListNode(-1);
        dummyHead.next = head;
        ListNode sortedList = head;
        ListNode cur = head.next;

        while (cur != null) {
            if (sortedList.val <= cur.val) {
                // 将 cur 插入到 sortedList 之后
                sortedList = sortedList.next;
            } else {
                ListNode prev = dummyHead;
                while (prev.next.val <= cur.val) {
                    prev = prev.next;
                }
                // 将 cur 插入到链表中间
                sortedList.next = cur.next;
                cur.next = prev.next;
                prev.next = cur;
            }
            cur = sortedList.next;
        }

        return dummyHead.next;
    }
    
    public ListNode sortList(ListNode head) {
        return insertionSort(head);
    }
}
```



## 归并排序

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode* next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* merge(ListNode* left, ListNode* right) {
        // 归并环节
        ListNode* dummyHead = new ListNode(-1);
        ListNode* cur = dummyHead;
        while (left && right) {
            if (left->val <= right->val) {
                cur->next = left;
                left = left->next;
            } else {
                cur->next = right;
                right = right->next;
            }
            cur = cur->next;
        }

        if (left) {
            cur->next = left;
        } else if (right) {
            cur->next = right;
        }

        return dummyHead->next;
    }

    ListNode* mergeSort(ListNode* head) {
        // 分割环节
        if (!head || !head->next) {
            return head;
        }

        // 快慢指针找到中心链节点
        ListNode* slow = head;
        ListNode* fast = head->next;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // 断开左右链节点
        ListNode* leftHead = head;
        ListNode* rightHead = slow->next;
        slow->next = nullptr;

        // 归并操作
        return merge(mergeSort(leftHead), mergeSort(rightHead));
    }
    
    ListNode* sortList(ListNode* head) {
        return mergeSort(head);
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode merge(ListNode left, ListNode right) {
        // 归并环节
        ListNode dummyHead = new ListNode(-1);
        ListNode cur = dummyHead;
        while (left != null && right != null) {
            if (left.val <= right.val) {
                cur.next = left;
                left = left.next;
            } else {
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }

        if (left != null) {
            cur.next = left;
        } else if (right != null) {
            cur.next = right;
        }

        return dummyHead.next;
    }

    public ListNode mergeSort(ListNode head) {
        // 分割环节
        if (head == null || head.next == null) {
            return head;
        }

        // 快慢指针找到中心链节点
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // 断开左右链节点
        ListNode leftHead = head;
        ListNode rightHead = slow.next;
        slow.next = null;

        // 归并操作
        return merge(mergeSort(leftHead), mergeSort(rightHead));
    }

    public ListNode sortList(ListNode head) {
        return mergeSort(head);
    }
}
```



## 快速排序

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode* next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* partition(ListNode* left, ListNode* right) {
        // 左闭右开，区间没有元素或者只有一个元素，直接返回第一个节点
        if (left == right || left->next == right) {
            return left;
        }
        // 选择头节点为基准节点
        int pivot = left->val;
        // 使用 nodeI, nodeJ 双指针，保证 nodeI 之前的节点值都小于基准节点值，
        // nodeI 与 nodeJ 之间的节点值都大于等于基准节点值
        ListNode* nodeI = left;
        ListNode* nodeJ = left->next;

        while (nodeJ != right) {
            // 发现一个小于基准值的元素
            if (nodeJ->val < pivot) {
                // 因为 nodeI 之前节点都小于基准值，所以先将 nodeI 向右移动一位
                // 此时 nodeI 节点值大于等于基准节点值
                nodeI = nodeI->next;
                // 将小于基准值的元素 nodeJ 与当前 nodeI 换位，
                // 换位后可以保证 nodeI 之前的节点都小于基准节点值
                int temp = nodeI->val;
                nodeI->val = nodeJ->val;
                nodeJ->val = temp;
            }
            nodeJ = nodeJ->next;
        }
        // 将基准节点放到正确位置上
        int temp = left->val;
        left->val = nodeI->val;
        nodeI->val = temp;
        return nodeI;
    }

    ListNode* quickSort(ListNode* left, ListNode* right) {
        if (left == right || left->next == right) {
            return left;
        }
        ListNode* pi = partition(left, right);
        quickSort(left, pi);
        quickSort(pi->next, right);
        return left;
    }

    ListNode* sortList(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }
        return quickSort(head, nullptr);
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode partition(ListNode left, ListNode right) {
        // 左闭右开，区间没有元素或者只有一个元素，直接返回第一个节点
        if (left == right || left.next == right) {
            return left;
        }
        // 选择头节点为基准节点
        int pivot = left.val;
        // 使用 nodeI, nodeJ 双指针，保证 nodeI 之前的节点值都小于基准节点值，
        // nodeI 与 nodeJ 之间的节点值都大于等于基准节点值
        ListNode nodeI = left;
        ListNode nodeJ = left.next;

        while (nodeJ != right) {
            // 发现一个小于基准值的元素
            if (nodeJ.val < pivot) {
                // 因为 nodeI 之前的节点都小于基准值，所以先将 nodeI 向右移动一位
                // 此时 nodeI 节点值大于等于基准节点值
                nodeI = nodeI.next;
                // 将小于基准值的元素 nodeJ 与当前 nodeI 换位，
                // 换位后可以保证 nodeI 之前的节点都小于基准节点值
                int temp = nodeI.val;
                nodeI.val = nodeJ.val;
                nodeJ.val = temp;
            }
            nodeJ = nodeJ.next;
        }
        // 将基准节点放到正确位置上
        int temp = left.val;
        left.val = nodeI.val;
        nodeI.val = temp;
        return nodeI;
    }

    public ListNode quickSort(ListNode left, ListNode right) {
        if (left == right || left.next == right) {
            return left;
        }
        ListNode pi = partition(left, right);
        quickSort(left, pi);
        quickSort(pi.next, right);
        return left;
    }

    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        return quickSort(head, null);
    }
}
```



## 计数排序

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode* next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* countingSort(ListNode* head) {
        if (!head) {
            return head;
        }

        // 找出链表中最大值 listMax 和最小值 listMin
        int listMin = INT_MAX;
        int listMax = INT_MIN;
        ListNode* cur = head;
        while (cur) {
            listMin = std::min(listMin, cur->val);
            listMax = std::max(listMax, cur->val);
            cur = cur->next;
        }

        int size = listMax - listMin + 1;
        vector<int> counts(size, 0);

        cur = head;
        while (cur) {
            counts[cur->val - listMin]++;
            cur = cur->next;
        }

        ListNode* dummyHead = new ListNode(-1);
        cur = dummyHead;
        for (int i = 0; i < size; i++) {
            while (counts[i] > 0) {
                cur->next = new ListNode(i + listMin);
                counts[i]--;
                cur = cur->next;
            }
        }

        return dummyHead->next;
    }

    ListNode* sortList(ListNode* head) {
        return countingSort(head);
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
public class Solution {
    public ListNode countingSort(ListNode head) {
        if (head == null) {
            return head;
        }

        // 找出链表中最大值 listMax 和最小值 listMin
        int listMin = Integer.MAX_VALUE;
        int listMax = Integer.MIN_VALUE;
        ListNode cur = head;
        while (cur != null) {
            listMin = Math.min(listMin, cur.val);
            listMax = Math.max(listMax, cur.val);
            cur = cur.next;
        }

        int size = listMax - listMin + 1;
        int[] counts = new int[size];

        cur = head;
        while (cur != null) {
            counts[cur.val - listMin]++;
            cur = cur.next;
        }

        ListNode dummyHead = new ListNode(-1);
        cur = dummyHead;
        for (int i = 0; i < size; i++) {
            while (counts[i] > 0) {
                cur.next = new ListNode(i + listMin);
                counts[i]--;
                cur = cur.next;
            }
        }

        return dummyHead.next;
    }

    public ListNode sortList(ListNode head) {
        return countingSort(head);
    }
}
```



## 桶排序

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode* next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    // 将链表节点值 val 添加到对应桶 buckets[index] 中
    void insertion(vector<ListNode*>& buckets, int index, int val) {
        if (!buckets[index]) {
            buckets[index] = new ListNode(val);
            return;
        }

        ListNode* node = new ListNode(val);
        node->next = buckets[index];
        buckets[index] = node;
    }

    // 归并环节
    ListNode* merge(ListNode* left, ListNode* right) {
        ListNode dummyHead(-1);
        ListNode* cur = &dummyHead;
        while (left && right) {
            if (left->val <= right->val) {
                cur->next = left;
                left = left->next;
            } else {
                cur->next = right;
                right = right->next;
            }
            cur = cur->next;
        }

        if (left) {
            cur->next = left;
        } else if (right) {
            cur->next = right;
        }

        return dummyHead.next;
    }

    ListNode* mergeSort(ListNode* head) {
        // 分割环节
        if (!head || !head->next) {
            return head;
        }

        // 快慢指针找到中心链节点
        ListNode* slow = head;
        ListNode* fast = head->next;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // 断开左右链节点
        ListNode* leftHead = head;
        ListNode* rightHead = slow->next;
        slow->next = nullptr;

        // 归并操作
        return merge(mergeSort(leftHead), mergeSort(rightHead));
    }

    ListNode* bucketSort(ListNode* head, int bucketSize = 5) {
        if (!head) {
            return head;
        }

        // 找出链表中最大值 listMax 和最小值 listMin
        int listMin = INT_MAX;
        int listMax = INT_MIN;
        ListNode* cur = head;
        while (cur) {
            listMin = min(listMin, cur->val);
            listMax = max(listMax, cur->val);
            cur = cur->next;
        }

        // 计算桶的个数，并定义桶
        int bucketCount = (listMax - listMin) / bucketSize + 1;
        vector<ListNode*> buckets(bucketCount, nullptr);

        // 将链表节点值依次添加到对应桶中
        cur = head;
        while (cur) {
            int index = (cur->val - listMin) / bucketSize;
            insertion(buckets, index, cur->val);
            cur = cur->next;
        }

        ListNode dummyHead(-1);
        cur = &dummyHead;
        // 将元素依次出桶，并拼接成有序链表
        for (ListNode* bucketHead : buckets) {
            ListNode* bucketCur = mergeSort(bucketHead);
            while (bucketCur) {
                cur->next = bucketCur;
                cur = cur->next;
                bucketCur = bucketCur->next;
            }
        }

        return dummyHead.next;
    }

    ListNode* sortList(ListNode* head) {
        return bucketSort(head);
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    // 将链表节点值 val 添加到对应桶 buckets[index] 中
    private void insertion(ListNode[] buckets, int index, int val) {
        if (buckets[index] == null) {
            buckets[index] = new ListNode(val);
            return;
        }

        ListNode node = new ListNode(val);
        node.next = buckets[index];
        buckets[index] = node;
    }

    // 归并环节
    private ListNode merge(ListNode left, ListNode right) {
        ListNode dummyHead = new ListNode(-1);
        ListNode cur = dummyHead;
        while (left != null && right != null) {
            if (left.val <= right.val) {
                cur.next = left;
                left = left.next;
            } else {
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }

        if (left != null) {
            cur.next = left;
        } else if (right != null) {
            cur.next = right;
        }

        return dummyHead.next;
    }

    private ListNode mergeSort(ListNode head) {
        // 分割环节
        if (head == null || head.next == null) {
            return head;
        }

        // 快慢指针找到中心链节点
        ListNode slow = head, fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // 断开左右链节点
        ListNode leftHead = head, rightHead = slow.next;
        slow.next = null;

        // 归并操作
        return merge(mergeSort(leftHead), mergeSort(rightHead));
    }

    public ListNode sortList(ListNode head) {
        if (head == null) {
            return head;
        }

        // 找出链表中最大值 listMax 和最小值 listMin
        int listMin = Integer.MAX_VALUE;
        int listMax = Integer.MIN_VALUE;
        ListNode cur = head;
        while (cur != null) {
            listMin = Math.min(listMin, cur.val);
            listMax = Math.max(listMax, cur.val);
            cur = cur.next;
        }

        // 计算桶的个数，并定义桶
        int bucketCount = (listMax - listMin) / 5 + 1;
        ListNode[] buckets = new ListNode[bucketCount];

        // 将链表节点值依次添加到对应桶中
        cur = head;
        while (cur != null) {
            int index = (cur.val - listMin) / 5;
            insertion(buckets, index, cur.val);
            cur = cur.next;
        }

        ListNode dummyHead = new ListNode(-1);
        cur = dummyHead;
        // 将元素依次出桶，并拼接成有序链表
        for (ListNode bucketHead : buckets) {
            ListNode bucketCur = mergeSort(bucketHead);
            while (bucketCur != null) {
                cur.next = bucketCur;
                cur = cur.next;
                bucketCur = bucketCur.next;
            }
        }

        return dummyHead.next;
    }
}
```



## 基数排序

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode* next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    int getMaxBit(ListNode* head) {
        int maxBit = 0;
        ListNode* cur = head;
        while (cur) {
            int valLen = 0;
            int val = cur->val;
            while (val > 0) {
                valLen++;
                val /= 10;
            }
            maxBit = max(maxBit, valLen);
            cur = cur->next;
        }
        return maxBit;
    }

    ListNode* radixSort(ListNode* head) {
        int maxBit = getMaxBit(head);

        for (int i = 0; i < maxBit; i++) {
            vector<ListNode*> buckets(10, nullptr);
            ListNode dummyHead(-1);
            ListNode* cur = head;

            while (cur) {
                int num = cur->val / static_cast<int>(pow(10, i)) % 10;
                if (!buckets[num]) {
                    buckets[num] = new ListNode(cur->val);
                } else {
                    ListNode* node = new ListNode(cur->val);
                    node->next = buckets[num];
                    buckets[num] = node;
                }
                cur = cur->next;
            }

            head = nullptr;
            ListNode* tail = &dummyHead;
            for (int j = 0; j < 10; j++) {
                if (buckets[j]) {
                    tail->next = buckets[j];
                    while (tail->next) {
                        tail = tail->next;
                    }
                }
            }

            head = dummyHead.next;
        }

        return head;
    }

    ListNode* sortList(ListNode* head) {
        return radixSort(head);
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
public class Solution {
    public int getMaxBit(ListNode head) {
        int maxBit = 0;
        ListNode cur = head;
        while (cur != null) {
            int valLen = 0;
            int val = cur.val;
            while (val > 0) {
                valLen++;
                val /= 10;
            }
            maxBit = Math.max(maxBit, valLen);
            cur = cur.next;
        }
        return maxBit;
    }

    public ListNode radixSort(ListNode head) {
        int maxBit = getMaxBit(head);

        for (int i = 0; i < maxBit; i++) {
            List<ListNode> buckets = new ArrayList<>();
            for (int j = 0; j < 10; j++) {
                buckets.add(null);
            }

            ListNode dummyHead = new ListNode(-1);
            ListNode cur = head;

            while (cur != null) {
                int num = cur.val / (int) Math.pow(10, i) % 10;
                if (buckets.get(num) == null) {
                    buckets.set(num, new ListNode(cur.val));
                } else {
                    ListNode node = new ListNode(cur.val);
                    node.next = buckets.get(num);
                    buckets.set(num, node);
                }
                cur = cur.next;
            }

            head = null;
            ListNode tail = dummyHead;
            for (int j = 0; j < 10; j++) {
                if (buckets.get(j) != null) {
                    tail.next = buckets.get(j);
                    while (tail.next != null) {
                        tail = tail.next;
                    }
                }
            }

            head = dummyHead.next;
        }

        return head;
    }

    public ListNode sortList(ListNode head) {
        return radixSort(head);
    }
}
```













































































