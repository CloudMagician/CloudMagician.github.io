---
layout:	post
title:	"Divide and Conquer"
image:	''
date:	2018-12-25 15:29:03
tags:	
- Algorithm
- Exam
description: 'My life for pass!'
categories:
- Algorithm
---

<script type="text/javascript" src="../MathJax/MathJax.js?config=default"></script>

# 求解思想

将整个问题分成若干个小问题后分而治之。通常，由分治法所得到的子问题与原问题具有相同的类型。可以反复使用分治策略，直到可以直接求解子问题为止。适合采用递归过程来表示。

# 计算时间

分治策略的计算时间可表示为：$$T(n)=\begin{cases}g(n)&\text{$n$足够小}\\2T(n/2)+f(n)&\text{否则}\end{cases}$$

* **T(n)**是输入规模为**n**的分治策略的计算时间。

* **g(n)**是对足够小的输入规模能直接计算出答案的时间。

* **f(n)**是**COMBINE**函数合成原问题解的计算时间。

---

**Example：**

**Q：**在下列情况下求解递归关系式：

$$T(n)=\begin{cases}g(n)&\text{$n$足够小}\\2T(n/2)+f(n)&\text{否则}\end{cases}$$

1. $$g(n)=O(1)$$ 和 $$f(n)=O(n)$$；
2. $$g(n)=O(1)$$ 和 $$f(n)=O(1)$$。

**A：**

1. 不妨设 $$g(n)=a$$ ， $$f(n)=bn$$ ，a、b为常数。

   令 $$n=2^k$$ ，则：

   $$\begin{align*}T(n)&=2T(n/2)+bn\\&=4T(n/4)+2bn\\&=...\\&=2^kT(n/2^k)+kbn\\&=an+bn\log_2n\\&=O(n\log_2n)\end{align*}$$ 

2. 不妨设 $$g(n)=a$$ ， $$f(n)=b$$ ，a、b为常数。

   令 $$n=2^k$$ ，则：

   $$\begin{align*}T(n)&=2T(n/2)+b\\&=4T(n/4)+2b\\&=...\\&=2^kT(n/2^k)+kb\\&=an+b\log_2n\\&=O(n)\end{align*}$$ 

---

# 二分检索

## 算法

```c
int BinarySearch(int* arr,int fin) {
    int left,right,mid;
    left = 0;
    right = sizeof(arr) / sizeof(arr[0]) - 1;
    
    while (left <= right) {
        mid = (left + right) / 2;
        if (arr[mid] == fin) {
            return mid;
        }
        if (mid < fin) {
            left = mid + 1;
        } else if (mid > fin) {
            right = mid - 1;
        }
    }
    
    return -1;  //not found
}
```

## 时空复杂度

* 空间：$$n+\text{变量数}$$
* 时间：任何一种以比较为基础的算法，其最坏情况下的计算时间都不可能低于 $$\Theta(\log n)$$ ，也就是不可能存在其最坏情况下计算时间比二分检索算法的计算时间数量级还低的算法。

| 计算时间 |      最好情况      |      平均情况      |      最坏情况      |
| :------: | :----------------: | :----------------: | :----------------: |
|   成功   |   $$\Theta(1)$$    | $$\Theta(\log n)$$ | $$\Theta(\log n)$$ |
|   失败   | $$\Theta(\log n)$$ | $$\Theta(\log n)$$ | $$\Theta(\log n)$$ |

# 归并分类

## 算法

* 从上往下的归并排序

```c
/*
 * 将一个数组中的两个相邻有序区间合并成一个
 *
 * 参数说明：
 *     a -- 包含两个有序区间的数组
 *     start -- 第1个有序区间的起始地址。
 *     mid   -- 第1个有序区间的结束地址。也是第2个有序区间的起始地址。
 *     end   -- 第2个有序区间的结束地址。
 */
void merge(int a[], int start, int mid, int end)
{
    int *tmp = (int *)malloc((end-start+1)*sizeof(int));    // tmp是汇总2个有序区的临时区域
    int i = start;            // 第1个有序区的索引
    int j = mid + 1;        // 第2个有序区的索引
    int k = 0;                // 临时区域的索引

    while(i <= mid && j <= end)
    {
        if (a[i] <= a[j])
            tmp[k++] = a[i++];
        else
            tmp[k++] = a[j++];
    }

    while(i <= mid)
        tmp[k++] = a[i++];

    while(j <= end)
        tmp[k++] = a[j++];

    // 将排序后的元素，全部都整合到数组a中。
    for (i = 0; i < k; i++)
        a[start + i] = tmp[i];

    free(tmp);
}

/*
 * 归并排序(从上往下)
 *
 * 参数说明：
 *     a -- 待排序的数组
 *     start -- 数组的起始地址
 *     endi -- 数组的结束地址
 */
void merge_sort_up2down(int a[], int start, int end)
{
    if(a==NULL || start >= end)
        return ;

    int mid = (end + start)/2;
    merge_sort_up2down(a, start, mid); // 递归排序a[start...mid]
    merge_sort_up2down(a, mid+1, end); // 递归排序a[mid+1...end]

    // a[start...mid] 和 a[mid...end]是两个有序空间，
    // 将它们排序成一个有序空间a[start...end]
    merge(a, start, mid, end);
}
```

* 从下而上的归并排序

```c
/*
 * 对数组a做若干次合并：数组a的总长度为len，将它分为若干个长度为gap的子数组；
 *             将"每2个相邻的子数组" 进行合并排序。
 *
 * 参数说明：
 *     a -- 待排序的数组
 *     len -- 数组的长度
 *     gap -- 子数组的长度
 */
void merge_groups(int a[], int len, int gap)
{
    int i;
    int twolen = 2 * gap;    // 两个相邻的子数组的长度

    // 将"每2个相邻的子数组" 进行合并排序。
    for(i = 0; i+2*gap-1 < len; i+=(2*gap))
    {
        merge(a, i, i+gap-1, i+2*gap-1);
    }

    // 若 i+gap-1 < len-1，则剩余一个子数组没有配对。
    // 将该子数组合并到已排序的数组中。
    if ( i+gap-1 < len-1)
    {
        merge(a, i, i + gap - 1, len - 1);
    }
}

/*
 * 归并排序(从下往上)
 *
 * 参数说明：
 *     a -- 待排序的数组
 *     len -- 数组的长度
 */
void merge_sort_down2up(int a[], int len)
{
    int n;

    if (a==NULL || len<=0)
        return ;

    for(n = 1; n < len; n*=2)
        merge_groups(a, len, n);
}
```

## 时空复杂度

- 时间：任何以关键字比较为基础的分类算法，最坏情况下的时间下界都是 $$\Omega(n\log n)$$ ，因此从数量级的角度上看**,** 归并算法是最坏情况下的最优算法。

# 斯塔拉森算法

## 思想

假定矩阵**A**和**B**的级数**n**是**2**的幂，否则，添加适当的全零行和全零列，使其变成级是**2**的幂的方阵。将**A**和**B**都分成**4**个**(n/2)\*(n/2)**的方形矩阵，于是**A**和**B**就可以看成是两个以**(n/2)\*(n/2)**矩阵为元素的**2\*2**矩阵。

## 复杂度

$$O(n^{\log 7})=O(n^{2.81})$$