## Sort

![c++](https://img.shields.io/badge/language-c%2B%2B-orange.svg)

### 1. 快速排序

> 快速排序（Quicksort）是对[冒泡排序](https://baike.baidu.com/item/冒泡排序/4602306)的一种改进。
>
> 快速排序由C. A. R. Hoare在1960年提出。它的基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以[递归](https://baike.baidu.com/item/递归/1740695)进行，以此达到整个数据变成有序[序列](https://baike.baidu.com/item/序列/1302588)。

#### c++递归版本

```cpp
template<typename T>
void quick_sort_recursive(T arr[], int left, int right){

    if (left >= right)
        return;

    int i = left, j = right;
    T key = arr[left];

    while (i < j){
        while (i < j && arr[j] > key)
            j--;
        arr[i] = arr[j];
        while (i < j && arr[i] <= key)
            i++;
        arr[j] = arr[i];
    }

    arr[i] = key;
    quick_sort(arr, left, i - 1);
    quick_sort(arr, i + 1, right);
}
```

#### c++迭代版本

```cpp
struct Range{
        int start, end;
        Range(int s = 0, int e = 0){
            start = s;
            end = e;
        }
    };

template <typename T>
void quick_sort(T arr[], const int len){
    if (len <= 0)
        return;

    Range r[len];

    int p = 0;
    r[p++] = Range(0, len - 1);
    while (p){
        Range range = r[--p];
        if (range.start >= range.end)
            continue;
        T key = arr[range.start];

        int left = range.start, right = range.end;
        while (left < right){
            while (arr[right] > key && right > left)
                right--;
            while (arr[left] <= key && right > left)
                left++;
            swap(arr[left], arr[right]);
        }
        if (arr[left] <= arr[range.start])
            swap(arr[left], arr[range.start]);
        else left--;
        r[p++] = Range(range.start, left - 1);
        r[p++] = Range(left + 1, range.end);
    }
}
```



