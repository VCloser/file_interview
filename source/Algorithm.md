## Algorithm

### Fibonacci

~~~python
# 求斐波那契的第n项

# 递归版本的斐波那契方法

import time
from sys import setrecursionlimit  # 设置递归的最大深度
setrecursionlimit(100000)  # 设置最大的递归深度


# 计算时间的装饰器
def cal_time(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print('%s running time: %s sec.' % (func.__name__, end_time - start_time))
        return result
    return wrapper

# 递归要比循环慢
# 时间复杂度O(2^n) 慢的原因就是重复计算
def fibonacci(n):
    if n == 0 or n == 1:
        return 1
    else:
        return fibonacci(n-1) + fibonacci(n-2)

# 使用循环的方式
# 时间复杂度O(n)
def fibonacci2(n):
    lst = [1, 1]
    for i in range(2, n+1):
        lst.append(lst[-1] + lst[-2])
    return lst[n]

# 递归的使用
# 时间复杂度O(n)
def fibonacci3(n):
    lst = [-1 for i in range(n + 1)]  # 把计算过的fibonacci数存贮起来
    def fib(n):
        if n == 0 or n == 1:
            return 1
        elif lst[n] >= 0:  # 直接返回
            return lst[n]
        else:
            v = fib(n-1) + fib(n-2)
            lst[n] = v
            return lst[n]
    return fib(n)

# 空间也节省的斐波那契
# 时间复杂度O(n)  空间复杂度O(1)
def fibonacci4(n):
    a = 1
    b = 1
    c = 1
    for i in range(2, n + 1):
        c = a + b
        a = b
        b = c
    return c




# 套层壳,解决递归问题中一直调用装饰器的问题
@cal_time
def fib(n):
    return fibonacci3(n)


# 求前十个斐波那契组成的列表
ret = [fibonacci4(i) for i in range(10)]
print(ret)
~~~

### Hanoi

~~~python
def hanoi(n, A, B, C):
    if n > 0:
        hanoi(n - 1, A, C, B)
        print('%s -> %s' % (A, C))
        hanoi(n - 1, B, A, C)

# hanoi(3, 'A', 'B', 'C')
# 需要的步数
# 2(n-1) + 1
~~~

### Bin_search

~~~python
def binary_search(li, val):
    low = 0
    high = len(li)-1
    while low <= high: # 只要候选区有值
        mid = (low + high) // 2
        if val == li[mid]:
            return mid
        elif val < li[mid]:
            high = mid - 1
        else: # val > li[mid]
            low = mid + 1
    return -1
~~~

### Bubble_sort

~~~python


def bubble_sort(li):
    for i in range(len(li) - 1):  # 一共需要排n-1次
        for j in range(len(li) - i - 1):  #
            if li[j] > li[j + 1]:
                li[j], li[j + 1] = li[j + 1], li[j]


# 优化后的冒泡排序
def bubble_sort_1(li):
    for i in range(len(li) - 1):
        exchange = False
        for j in range(len(li) - i - 1):
            if li[j] > li[j + 1]:
                li[j], li[j + 1] = li[j + 1], li[j]
                exchange = True
        if not exchange:
            return

# 冒泡排序最好情况的时间复杂度度是O(n)
# 冒泡排序平均情况的时间复杂度度是O(n^2)
# 冒泡排序最灰坏情况的时间复杂度度是O(n^2)
~~~

### Select_sort

~~~python


def select_sort(li):
    for i in range(len(li) - 1):
        # 第i趟  无序区【i, len(li) - 1】
        # 找到无序区最小的位置，和无序区第一个数交换
        min_pos = i
        for j in range(i + 1, len(li)):
            if li[j] < li[min_pos]:
                min_pos = j
        li[i], li[min_pos] = li[min_pos], li[i]

# 时间复杂度O(n^2)
~~~

### Insert_sort

~~~python
def insert_sort(li):
    for i in range(1, len(li)):
        tmp = li[i]
        j = i - 1
        while j >= 0 and li[j] > tmp:
            li[j + 1] = li[j]
            j -= 1
        li[j + 1] = tmp
~~~

### Quick_sort

~~~python
def quick_sort(li, left, right):
    if left < right:
        mid = partition(li, left, right)
        quick_sort(li, left, mid - 1)
        quick_sort(li, mid + 1, right)

def partition(li, left, right):
    tmp = li[left]
    while left < right:
        while left < right and li[right] >= tmp:
            right -= 1
        li[left] = li[right]
        while left < right and li[left] <= tmp:
            left += 1
        li[right] = li[left]
    li[left] = tmp
    return left
~~~

### Heap_sort

~~~python
def sift(li, low, high):  # 堆向下调整的性质
    # low表示堆顶下标，high表示最后一个元素的下标
    tmp = li[low]
    i = low
    j = 2 * i + 1
    while j <= high:  # 第二种循环退出的情况，没有没有孩子和tmp争抢i这个位置
        if j < high and li[j + 1] > li[j]:  # 如果右孩子存在并且比左孩子大 j 指向右孩子
            j += 1
        if li[j] > tmp:
            li[i] = li[j]
            i = j
            j = 2 * i + 1
        else:
            break  # 第一种退出的情况， tmp比目前两个孩子都打大
    li[i] = tmp


def heap_sort(li):
    # 1. 从列表构建堆，主要维护low和high的值
    n = len(li)
    for low in range(n // 2 - 1, -1, -1):
        sift(li, low, n - 1)
    # 挨个出数 利用原来的的空间存贮下来的数据， 但是这个数不属于堆
    for high in range(n - 1, -1, -1):  # 或着是range(n-1, 0, -1)
        li[high], li[0] = li[0], li[high]  # 退休，调整
        sift(li, 0, high - 1)


# s时间复杂度O(nlogn)

# 可以使用内置的heaoq模块

import heapq

li = [9, 8, 5, 4, 7, 6, 3, 1, 2]
heapq.heapify(li)  # 建立堆
print(li)
heapq.heappush(li, 0)  # 添加堆
print(li)
heapq.heappop(li)  # 弹出堆
print(li)
# 使用内置的方法进行堆的排序
result = [heapq.heappop(li) for i in range(len(li))]
print(result)

# topk问题
def sift_topk(li, low, high):  # 堆向下调整的性质
    # low表示堆顶下标，high表示最后一个元素的下标
    tmp = li[low]
    i = low
    j = 2 * i + 1
    while j <= high:  # 第二种循环退出的情况，没有没有孩子和tmp争抢i这个位置
        if j < high and li[j + 1] < li[j]:  # 如果右孩子存在并且比左孩子大 j 指向右孩子
            j += 1
        if li[j] < tmp:
            li[i] = li[j]
            i = j
            j = 2 * i + 1
        else:
            break  # 第一种退出的情况， tmp比目前两个孩子都打大
    li[i] = tmp


def topk(li, k):
    heap = li[0: k]
    for i in range(k // 2 - 1, -1, -1):
        sift_topk(heap, i, k - 1)
    for j in range(k, len(li)):
        if li[j] > heap[0]:
            sift_topk(li, 0, k - 1)
    for k in range(k - 1, -1, -1):
        heap[0], heap[k] = heap[k], heap[0]
        sift_topk(li, 0, k -1 )
# 时间复杂度
# klogk + (n-k)logk
# 最后的时间复杂度是O(nlogk)


# 使用内置的heapq找topk
import random
lst = list(range(10000))
random.shuffle(lst)
# 最大的几个数
ret = heapq.nlargest(5, lst)
print(ret)
# 最小的几个数
small = heapq.nsmallest(5, lst)
print(small)
~~~

### Merge_sort

~~~python
# 时间复杂度死0(n)
def merge(li, low, mid, high):
    i = low
    j = mid + 1
    li_tmp = []
    while i <= mid and j <= high:  # 两边都有数
        if li[i] <= li[j]:
            li_tmp.append(i)
            i += 1
        else:
            li_tmp.append(j)
            j += 1
    # i <= mid and j <= high 两个条件只能由一个满足
    while i <= mid:
        li_tmp.append(li[i])
        i += 1
    while j <= high:
        li_tmp.append(j)
        j += 1
    # 将列表的代码拷贝回去
    for i in range(len(li_tmp)):
        li[low + i] = li_tmp[i]


# 时间复杂度是O(logn)
def merge_sort(li, low, high):
    if low < high:
        mid = (low + high) // 2
        merge_sort(li, low, mid)
        merge_sort(li, mid + 1, high)
        merge(li, low, mid, high)


# 总体的时间复杂度是O(nlogn)
li = [10, 4, 6, 3, 8, 2, 5, 7]
merge_sort(li, 0, len(li) - 1)
print(li)
# Python内置sort使用的是Timsort
~~~

