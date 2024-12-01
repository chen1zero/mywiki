# 排序算法

## 冒泡排序

排序原理：

1. 比较相邻的元素。如果前一个元素比后一个元素大，就交换这两个元素的位置。
2. 从开始到结尾对每一对相邻元素重复步骤1。最终最后位置的元素就是最大值。

```python
'''
冒泡排序 
O(N^2)
'''
def bubblesort(alist):
    for i in range(len(alist)-1, 0, -1):    #n-1趟比较交换
        for j in range(i):
            if alist[j]>alist[j+1]:
                alist[j], alist[j+1] = alist[j+1], alist[j]
                
alist=[54,26,93,17,77,31,44,55,20]
bubblesort(alist)
print(alist)
```

```python
'''
冒泡排序改进:如果某趟比对过程中没有发生过交换，说明已经排好序了，可以提前结束算法
'''
def shortbubblesort(alist):
    i=len(alist)-1 #n-1趟比较交换
    exchange=True
    while i>0 and exchange:
        exchange=False
        for j in range(i):
            if alist[j]>alist[j+1]:
                alist[j], alist[j+1] = alist[j+1], alist[j]
                exchange=True
        i = i - 1
```



## 选择排序

排序原理：
1.每一次遍历的过程中，都假定最后一个索引处的元素是最大值，和其他索引处的值依次进行比较，如果当前索引处的值小于其他某个索引处的值，则假定其他某个索引出的值为最大值，最后可以找到最大值所在的索引
2.交换最后一个索引处和最大值所在的索引处的值

```python
'''
选择排序；每趟仅进行一次交换，记录最大项所在的位置，最后再跟本趟最后一项交换
O(N^2)
'''
def selectionsort(alist):
    for i in range(len(alist)-1, 0, -1):
        maxpos=i
        for j in range(i):
            if alist[j]>alist[maxpos]:
                maxpos=j
        alist[maxpos], alist[i] = alist[i], alist[maxpos]
```

## 插入排序

排序原理：
1.把所有的元素分为两组，已经排序的和未排序的；
2.找到未排序的组中的第一个元素，向已经排序的组中进行插入；
3.倒叙遍历已经排序的元素，依次和待插入的元素进行比较，直到找到一个元素小于等于待插入元素，那么就把待
插入元素放到这个位置，其他的元素向后移动一位；

```python
'''
插入排序
O(N^2)
'''
def insertionsort(alist):
    for i in range(1, len(alist)):
        currentvalue = alist[i]
        pos = i
        while pos>0:
            if alist[pos-1]>currentvalue:
                alist[pos] = alist[pos-1]  #比对移动,不断找新项的插入位置
                pos=pos-1
            else:
                break
        alist[pos]=currentvalue            #插入新项
```



## 希尔排序

希尔排序是插入排序的一种，又称“缩小增量排序”，是插入排序算法的一种更高效的改进版本。

排序原理：
1.选定一个增长量h，按照增长量h作为数据分组的依据，对数据进行分组；h一般从N/2开始，每趟减半直到1
2.对分好组的每一组数据完成插入排序；
3.减小增长量，最小减为1，重复第二步操作。

```python
'''
希尔排序
'''
def shellsort(alist):
    gap = len(alist)//2
    while gap>0:
        for startpos in range(gap):
            gapinsertionsort(alist, startpos, gap)
        gap = gap//2
def gapinsertionsort(alist, startpos, gap):
    for i in range(startpos+gap, len(alist), gap):
        pos = i
        currentvalue = alist[pos]
        while pos > startpos:
            if alist[pos-gap] > currentvalue:
                alist[pos] = alist[pos-gap]
                pos = pos-gap
            else:
                break
        alist[pos] = currentvalue
```



## 归并排序

排序原理：
1.尽可能的一组数据拆分成两个元素相等的子组，并对每一个子组继续拆分，直到拆分后的每个子组的元素个数是
1为止。
2.将相邻的两个子组进行合并成一个有序的大组；
3.不断的重复步骤2，直到最终只有一个组为止。

```python
'''
归并排序
O(NlogN)
'''
def mergesort(alist):
    if len(alist)==1:   #基本结束条件
        return alist
    mid = len(alist)//2
    lefthalf = alist[:mid]
    righthalf = alist[mid:]
    mergesort(lefthalf)
    mergesort(righthalf)  #递归调用
    ###下面将左右两部分的排序结果拉链式交错归并到结果列表中
    i,j,k=0
    while i<len(lefthalf) and j<len(righthalf):
        if lefthalf[i]<=righthalf[j]:
            alist[k++]=lefthalf[i++]
        else:
            alist[k++]=righthalf[j++]
    ##归并左半部分剩余项
    while i<len(lefthalf):
        alist[k++]=lefthalf[i++]
    ##归并右半部分剩余项
    while j<len(righthalf):
        alist[k++] = righthalf[j++]
```





## 快速排序

排序原理：

1.首先设定一个分界值，通过该分界值将数组分成左右两部分，一般设定分界值为数组的第一个元素；
2.将大于或等于分界值的数据放到到数组右边，小于分界值的数据放到数组的左边。此时左边部分中各元素都小于
或等于分界值，而右边部分中各元素都大于或等于分界值；                                                                                          3.然后，左边和右边的数据可以独立排序。对于左侧的数组数据，又可以取一个分界值，将该部分数据分成左右两
部分，同样在左边放置较小值，右边放置较大值。右侧的数组数据也可以做类似处理。
4.重复上述过程，可以看出，这是一个递归定义。通过递归将左侧部分排好序后，再递归排好右侧部分的顺序。当
左侧和右侧两个部分的数据排完序后，整个数组的排序也就完成了。

把一个数组切分成两个子数组的基本思想：
1.找一个基准值，用两个指针分别指向数组的头部和尾部；
2.先从尾部向头部开始搜索一个比基准值小的元素，搜索到即停止，并记录指针的位置；
3.再从头部向尾部开始搜索一个比基准值大的元素，搜索到即停止，并记录指针的位置；
4.交换当前左边指针位置和右边指针位置的元素；
5.重复2,3,4步骤，直到左边指针的值大于右边指针的值停止。

```python
'''
快速排序
O(NlogN)
'''

def quicksort(alist):
    quicksorthelper(alist, 0, len(alist)-1)

def quicksorthelper(alist, first, last):
    if first<last:
        midpoint = partition(alist, first, last)
        quicksorthelper(alist, first, midpoint-1)
        quicksorthelper(alist, midpoint+1, last)

def partition(alist, first, last):
    midvalue = alist[first]
    leftmark = first+1
    rightmark = last
    while True:
        while alist[rightmark] >= midvalue:
            rightmark = rightmark - 1
            if rightmark = fitst:
                break
        while alist[leftmark] <= midvalue:
            leftmark = leftmark + 1
            if leftmark == last:
                break
        if leftmark >= rightmark:
            break
        else：
            alist[leftmark],alist[rightmark] = alist[rightmark], alist[leftmark]
    alist[first], alist[rightmark] = alist[rightmark], alist[first]
    return rightmark

##测试代码
alist=[54,26,93,17,77,31,44,55,20]
quicksort(alist)
print(alist)
```



## 堆排序



## 排序算法稳定性

稳定性的定义：
数组arr中有若干元素，其中A元素和B元素相等，并且A元素在B元素前面，如果使用某种排序算法排序后，能够保证A元素依然在B元素的前面，可以说这个该算法是稳定的。

冒泡排序：
只有当arr[i]>arr[i+1]的时候，才会交换元素的位置，而相等的时候并不交换位置，所以冒泡排序是一种**稳定**排序
算法。
选择排序:
选择排序是给每个位置选择当前元素最小的,例如有数据{5(1)，8 ，5(2)， 2， 9 },第一遍选择到的最小元素为2，所以5(1)会和2进行交换位置，此时5(1)到了5(2)后面，破坏了稳定性，所以选择排序是一种**不稳定**的排序算法。
插入排序：
比较是从有序序列的末尾开始，也就是想要插入的元素和已经有序的最大者开始比起，如果比它大则直接插入在其后面，否则一直往前找直到找到它该插入的位置。如果碰见一个和插入元素相等的，那么把要插入的元素放在相等元素的后面。所以，相等元素的前后顺序没有改变，从原无序序列出去的顺序就是排好序后的顺序，所以插入排序是**稳定**的。
希尔排序：
希尔排序是按照不同步长对元素进行插入排序 ,虽然一次插入排序是稳定的，不会改变相同元素的相对顺序，但在不同的插入排序过程中，相同的元素可能在各自的插入排序中移动，最后其稳定性就会被打乱，所以希尔排序是**不稳定**的。
归并排序：
归并排序在归并的过程中，只有arr[i]<arr[i+1]的时候才会交换位置，如果两个元素相等则不会交换位置，所以它并不会破坏稳定性，归并排序是**稳定**的。
快速排序：
快速排序需要一个基准值，在基准值的右侧找一个比基准值小的元素，在基准值的左侧找一个比基准值大的元素，然后交换这两个元素，此时会破坏稳定性，所以快速排序是一种**不稳定**的算法。







