## Sort

![c++](https://img.shields.io/badge/language-c%2B%2B-orange.svg)

[TOC]

### 1. 快速排序

> 快速排序（Quicksort）是对[冒泡排序](https://baike.baidu.com/item/冒泡排序/4602306)的一种改进。
>
> 快速排序由C. A. R. Hoare在1960年提出。它的基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以[递归](https://baike.baidu.com/item/递归/1740695)进行，以此达到整个数据变成有序[序列](https://baike.baidu.com/item/序列/1302588)。

![快速排序1](assets/Sorting_quicksort_anim.gif)

| 最坏时间复杂度     | $O(n^2)$                     |
| ------------------ | ---------------------------- |
| **最优时间复杂度** | $O(n \log n)$                |
| **平均时间复杂度** | $O(n \log n)$                |
| **最坏空间复杂度** | **根据实现的方法不同而不同** |



> 快速排序使用[分治法](https://zh.wikipedia.org/wiki/分治法)（Divide and conquer）策略来把一个[序列](https://zh.wikipedia.org/wiki/序列)（list）分为较小和较大的2个子序列，然后递归地排序两个子序列。
>
> 步骤为：
>
> 1. 挑选基准值：从数列中挑出一个元素，称为“基准”（pivot），
> 2. 分割：重新排序数列，所有比基准值小的元素摆放在基准前面，所有比基准值大的元素摆在基准后面（与基准值相等的数可以到任何一边）。在这个分割结束之后，对基准值的排序就已经完成，
> 3. 递归排序子序列：[递归](https://zh.wikipedia.org/wiki/递归)地将小于基准值元素的子序列和大于基准值元素的子序列排序。
>
> 递归到最底部的判断条件是数列的大小是零或一，此时该数列显然已经有序。
>
> 选取基准值有数种具体方法，此选取方法对排序的时间性能有决定性影响。

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

### 2. 冒泡排序

> **冒泡排序**（英语：**Bubble Sort**）是一种简单的[排序算法](https://zh.wikipedia.org/wiki/排序算法)。它重复地走访过要排序的[数列](https://zh.wikipedia.org/wiki/数列)，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端

![冒泡排序1](assets/Bubble_sort_animation.gif)

| 最坏时间复杂度     | $O(n^2)$                             |
| ------------------ | ------------------------------------ |
| **最优时间复杂度** | $O(n)$                               |
| **平均时间复杂度** | $O(n^2)$                             |
| **最坏空间复杂度** | **总共**$O(n)$**需要辅助空间**$O(1)$ |

> 冒泡排序算法的运作如下：
>
> 1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
> 2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
> 3. 针对所有的元素重复以上的步骤，除了最后一个。
> 4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

![冒牌排序2](assets/512px-Bubblesort-edited-color.svg.png)

#### c++

```cpp
template <typename T>
void Bubble_sort(T arr[], int len){
    for (int i = 0; i < len - 1; i++){
        for (int j = 0; j < len - 1 - i; j++)
        {
            if (arr[j] > arr[j + 1])
                swap(arr[j], arr[j + 1]);

        }
    }
}
```

### 3.鸡尾酒排序

> **鸡尾酒排序**，也就是**定向冒泡排序**，**鸡尾酒搅拌排序**，**搅拌排序**（也可以视作[选择排序](https://zh.wikipedia.org/wiki/選擇排序)的一种变形），**涟漪排序**，**来回排序**或**快乐小时排序**，是[冒泡排序](https://zh.wikipedia.org/wiki/冒泡排序)的一种变形。此算法与[冒泡排序](https://zh.wikipedia.org/wiki/冒泡排序)的不同处在于排序时是以双向在序列中进行排序。

![鸡尾酒排序1](assets/Sorting_shaker_sort_anim.gif)

| 最坏时间复杂度     | $O(n^2)$ |
| ------------------ | -------- |
| **最优时间复杂度** | $O(n)$   |
| **平均时间复杂度** | $O(n^2)$ |

> 鸡尾酒排序等于是[冒泡排序](https://zh.wikipedia.org/wiki/冒泡排序)的轻微变形。不同的地方在于从低到高然后从高到低，而[冒泡排序](https://zh.wikipedia.org/wiki/冒泡排序)则仅从低到高去比较序列里的每个元素。他可以得到比[冒泡排序](https://zh.wikipedia.org/wiki/冒泡排序)稍微好一点的性能，原因是[冒泡排序](https://zh.wikipedia.org/wiki/冒泡排序)只从一个方向进行比对（由低到高），每次循环只移动一个项目。
>
> 以序列(2,3,4,5,1)为例，鸡尾酒排序只需要访问一次序列就可以完成排序，但如果使用[冒泡排序](https://zh.wikipedia.org/wiki/冒泡排序)则需要四次。但是在随机数序列的状态下，鸡尾酒排序与冒泡排序的效率都很差劲。

#### c++

```cpp
template <typename T>
void cocktail_sort(T arr[], int len){
    int left = 0, right = len - 1;
    while (left < right){
        for (int j = left; j < right; j++){
            if (arr[j] > arr[j + 1])
                swap(arr[j], arr[j + 1]);
        }
        right--;
        for (int j = right; j > left; j--){
            if (arr[j] < arr[j - 1])
                swap(arr[j], arr[j - 1]);
        }
        left++;
    }
}
```

### 4.选择排序

> **择排序**（Selection sort）是一种简单直观的[排序算法](https://zh.wikipedia.org/wiki/排序算法)。它的工作原理如下。首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

![选择排序1](assets/Selection_sort_animation.gif)

> 选择排序的主要优点与数据移动有关。如果某个元素位于正确的最终位置上，则它不会被移动。选择排序每次交换一对元素，它们当中至少有一个将被移到其最终位置上，因此对{\displaystyle n}![n](assets/a601995d55609f2d9f5e233e36fbe9ea26011b3b.svg)个元素的表进行排序总共进行至多{\displaystyle n-1}![{\displaystyle n-1}](assets/fbd0b0f32b28f51962943ee9ede4fb34198a2521.svg)次交换。在所有的完全依靠交换去移动元素的排序方法中，选择排序属于非常好的一种。

![选择排序2](assets/Selection-Sort-Animation.gif)

> 选择排序的示例动画。红色表示当前最小值，黄色表示已排序序列，蓝色表示当前位置。

#### c++

```cpp
template <typename T>
void selection_sort(T arr[], int len){
    int min;
    for (int i = 0; i < len - 1; i++){
        min = i;
        for (int j = i + 1; j < len; j++){
            if (arr[min] > arr[j])
                min = j;
        }
        swap(arr[min], arr[i]);
    }
}
```

### 5. 插入排序

> **插入排序**（英语：Insertion Sort）是一种简单直观的[排序算法](https://zh.wikipedia.org/wiki/排序算法)。它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。**插入排序**在实现上，通常采用in-place排序（即只需用到![{\displaystyle O(assets/e66384bc40452c5452f33563fe0e27e803b0cc21.svg)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e66384bc40452c5452f33563fe0e27e803b0cc21)的额外空间的排序），因而在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。

![插入排序1](assets/Insertion_sort_animation.gif)

| 最坏时间复杂度     | $O(n^2)$                             |
| ------------------ | ------------------------------------ |
| **最优时间复杂度** | $O(n)$                               |
| **平均时间复杂度** | $O(n^2)$                             |
| **最坏空间复杂度** | **总共**$O(n)$**需要辅助空间**$O(1)$ |

> 一般来说，**插入排序**都采用in-place在数组上实现。具体算法描述如下：
>
> 1. 从第一个元素开始，该元素可以认为已经被排序
> 2. 取出下一个元素，在已经排序的元素序列中从后向前扫描
> 3. 如果该元素（已排序）大于新元素，将该元素移到下一位置
> 4. 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
> 5. 将新元素插入到该位置后
> 6. 重复步骤2~5
>
> 如果*比较操作*的代价比*交换操作*大的话，可以采用[二分查找法](https://zh.wikipedia.org/wiki/二分查找法)来减少*比较操作*的数目。该算法可以认为是**插入排序**的一个变种，称为[二分查找插入排序](https://zh.wikipedia.org/w/index.php?title=二分查找插入排序&action=edit&redlink=1)。

![插入排序2](assets/220px-Insertion-sort-example-300px.gif)

> 使用插入排序为一列数字进行排序的过程

#### c++

```cpp
template <typename T>
void insert_sort(T arr[], int len){
    for (int i = 1; i < len; i++){
        int key = arr[i];
        int j = i - 1;
        while ((j >= 0) && (key < arr[j])){
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

### 6. 桶排序

> **桶排序（Bucket sort）**或所谓的**箱排序**，是一个[排序算法](https://zh.wikipedia.org/wiki/排序算法)，工作的原理是将[数组](https://zh.wikipedia.org/wiki/陣列)分到有限数量的桶里。每个桶再个别排序（有可能再使用别的[排序算法](https://zh.wikipedia.org/wiki/排序算法)或是以递归方式继续使用桶排序进行排序）。桶排序是[鸽巢排序](https://zh.wikipedia.org/wiki/鴿巢排序)的一种归纳结果。当要被排序的数组内的数值是均匀分配的时候，桶排序使用线性时间（{![{\displaystyle \Theta (assets/a6351206e27071559aa4472579095994f650d76b.svg)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/a6351206e27071559aa4472579095994f650d76b)（[大O符号](https://zh.wikipedia.org/wiki/大O符号)））。但桶排序并不是[比较排序](https://zh.wikipedia.org/wiki/比较排序)，他不受到{![{\displaystyle O(assets/9d2320768fb54880ca4356e61f60eb02a3f9d9f1.svg)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9d2320768fb54880ca4356e61f60eb02a3f9d9f1)下限的影响。

> 桶排序以下列程序进行：
>
> 1. 设置一个定量的数组当作空桶子。
> 2. 寻访序列，并且把项目一个一个放到对应的桶子去。
> 3. 对每个不是空的桶子进行排序。
> 4. 从不是空的桶子里把项目再放回原来的序列中。

| 最坏时间复杂度     | $O(n^2)$   |
| ------------------ | ---------- |
| **平均时间复杂度** | $O(n + k)$ |
| **最坏空间复杂度** | $O(n*k)$   |

#### c++

> 假设数据分布在[0，100)之间，每个桶内部用链表表示，在数据入桶的同时插入排序。然后把各个桶中的数据合并。

```cpp
//桶的数目
#define BUCKET_NUM 10
//链表
struct ListNode{
    //关闭自动转换特性
    /*
        使用了explicit关键字，则无法进行隐式转换。即：Demo test;test = 12.2;是无效的！但是我们		  可以进行显示类型转换，如：
            Demo test;
            test = Demo(12.2); 或者
            test = (Demo)12.2;
    */
    explicit ListNode(int i = 0):mData(i),mNext(NULL){}
    ListNode* mNext;
    int mData;
};
//每个桶内采用插入排序
ListNode* insert(ListNode* head, int val) {
    ListNode tempNode;
    ListNode *newNode = new ListNode(val);
    ListNode *pre, *curr;
    tempNode.mNext = head;
    pre = &tempNode;
    curr = head;
    while (curr != NULL && curr->mData <= val) {
        pre = curr;
        curr = curr->mNext;
    }
    newNode->mNext = curr;
    pre->mNext = newNode;
    return tempNode.mNext;
}
//合并两个桶
ListNode* Merge(ListNode *head1, ListNode *head2){
    ListNode tempNode;
    ListNode *temp = &tempNode;

    while (head1 != NULL && head2 != NULL){
        if (head1->mData <= head2->mData){
            temp->mNext = head1;
            head1 = head1->mNext;
        }else{
            temp->mNext = head2;
            head2 = head2->mNext;
        }
        temp = temp->mNext;
    }
    if (head1 != NULL) temp->mNext = head1;
    if (head2 != NULL) temp->mNext = head2;
    return tempNode.mNext;
}
//将数据分配到各个桶并进行合并，最后把数据取出，即为排完序的
void BuckSort(int n, int arr[]){
    vector<ListNode*> buckets(BUCKET_NUM, (ListNode*)(0));
    for (int i = 0; i < n; i++){
        int index = arr[i]/BUCKET_NUM;
        ListNode *head = buckets.at(index);
        buckets.at(index) = insert(head, arr[i]);
    }
    ListNode * head = buckets.at(0);
    for (int i = 1; i < BUCKET_NUM; i++){
        head = Merge(head, buckets.at(i));
    }
    for (int i = 0; i < n; i++){
        arr[i] = head->mData;
        head = head->mNext;
    }
}
```

### 7. 归并排序

> **归并排序**（英语：Merge sort，或mergesort），是创建在归并操作上的一种有效的[排序算法](https://zh.wikipedia.org/wiki/排序算法)，[效率](https://zh.wikipedia.org/wiki/时间复杂度)为![{\displaystyle O(assets/9d2320768fb54880ca4356e61f60eb02a3f9d9f1-1557402790597.svg)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9d2320768fb54880ca4356e61f60eb02a3f9d9f1)（[大O符号](https://zh.wikipedia.org/wiki/大O符号)）。1945年由[约翰·冯·诺伊曼](https://zh.wikipedia.org/wiki/约翰·冯·诺伊曼)首次提出。该算法是采用[分治法](https://zh.wikipedia.org/wiki/分治法)（Divide and Conquer）的一个非常典型的应用，且各层分治递归可以同时进行。

![归并排序1](assets/Merge_sort_animation2.gif)

| 最坏时间复杂度     | $O(n\log n)$   |
| ------------------ | -------------- |
| **最优时间复杂度** | $O(n  \log n)$ |
| **平均时间复杂度** | $O(n \log n)$  |
| **最坏空间复杂度** | $O(n)$         |

> ***概述***
>
> 采用分治法:
>
> - 分割：递归地把当前序列平均分割成两半。
> - 集成：在保持元素顺序的同时将上一步得到的子序列集成到一起（归并）。
>
> ***归并操作***
>
> 归并操作（merge），也叫归并算法，指的是将两个已经排序的序列合并成一个序列的操作。归并排序算法依赖归并操作。
>
> **递归法（Top-down）**
>
> 1. 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列
> 2. 设定两个指针，最初位置分别为两个已经排序序列的起始位置
> 3. 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置
> 4. 重复步骤3直到某一指针到达序列尾
> 5. 将另一序列剩下的所有元素直接复制到合并序列尾
>
> **迭代法（Bottom-up）**
>
> 原理如下（假设序列共有个元素）：
>
> 1. 将序列每相邻两个数字进行归并操作，形成![{\displaystyle ceil(assets/284284713ad8f1ba13458b896c87efc4b9b7df9c.svg)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/284284713ad8f1ba13458b896c87efc4b9b7df9c)个序列，排序后每个序列包含两/一个元素
> 2. 若此时序列数不是1个则将上述序列再次归并，形成![{\displaystyle ceil(assets/0f7b6be8e0c402e981a78d573dc3072c3d24a3c4.svg)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/0f7b6be8e0c402e981a78d573dc3072c3d24a3c4)个序列，每个序列包含四/三个元素
> 3. 重复步骤2，直到所有元素排序完毕，即序列数为1

![归并排序2](assets/220px-Merge-sort-example-300px.gif)

#### c++递归法

```cpp
void Merge(vector<int> &Array, int front, int mid, int end){
    vector<int> LeftSubArray(Array.begin() + front, Array. begin() + mid + 1);
    vector<int> RightSubArray(Array.begin() + mid + 1, Array.begin() + end + 1);

    int idxLeft = 0, idxRight = 0;
    LeftSubArray.insert(LeftSubArray.end(), numeric_limits<int>::max());
    RightSubArray.insert(RightSubArray.end(), numeric_limits<int>::max());

    for (int i = front; i <= end; i++){
        if (LeftSubArray[idxLeft] < RightSubArray[idxRight]){
            Array[i] = LeftSubArray[idxLeft];
            idxLeft++;
        }else{
            Array[i] = RightSubArray[idxRight];
            idxRight++;
        }
    }
}

void MergeSort(vector<int> &Array, int front, int end){
    if (front >= end) return;
    int mid = front + (end - front) / 2;
    MergeSort(Array, front, mid);
    MergeSort(Array, mid + 1, end);
    Merge(Array, front, mid, end);
}
```

#### c++迭代法

```cpp
template <typename T>
void merge_sort(T arr[], int len){
    T *a = arr;
    T *b = new T[len];
    for (int seg = 1; seg < len; seg+=seg){
        for (int start = 0; start < len; start += seg + seg){
            int low = start, mid = min(start + seg, len), high = min(start + seg + seg, len);
            int k = low;
            int start1 = low, end1 = mid;
            int start2 = mid, end2 = high;
            while (start1 < end1 && start2 < end2)
                b[k++] = a[start1] < a[start2]?a[start1++]:a[start2++];
            while (start1 < end1)
                b[k++] = a[start1++];
            while (start2 < end2)
                b[k++] = a[start2++];
        }
        T *temp = a;
        a = b;
        b = temp;
    }
    if (a != arr){
        for (int i = 0; i < len; i++)
            b[i] = a[i];
        b = a;
    }
    delete[] b;
}
```

