# Sort



## 十大经典排序算法

默认从小到大排哈。

### 1.Bubble Sort

**一句话总结：**冒泡排序。相邻数比较，大数下沉小数冒起。

第一轮：从第0个数开始挨个和自己下面的比较，最大的最后会沉底部

第二轮：从第0个数开始挨个和自己下面的比较，比到倒数第二个，最大的两个数会按顺序沉底。

C++代码：

```C++
void BubbleSort(){
	printf("Bubble Sort:\n");
	int testArray[] = {66, 8, 24, 12, 39, 48, 57};

	//.length()是string用的、数组长度用介个(sizeof(testArray)/sizeof(testArray[0]))算
	int arrLength = sizeof(testArray)/sizeof(testArray[0]);
    
	for(int i=0; i < (arrLength - 1); i++){				// 外圈循环是多少轮的计数。
		// 单轮比较的控制 比较次数 =  数组长度 - i(第i次) -1
		for(int j=0; j < (arrLength - i -1); j++){
			if(testArray[j] > testArray[j+1]){		// 比较：要是比自己下一个数字大就和下一个数交换位置
				testArray[j] ^= testArray[j+1];		// 注意是"^="。不可以写"^ ="（中间带空格）呀。
				testArray[j+1] ^= testArray[j];
				testArray[j] ^= testArray[j+1];
			}
		}
	}

	// 结果输出
	for(int i=0; i < arrLength; i++){
		printf("%d\n",testArray[i]);
	}

	// 数组越界警告：如果出现下面这个异常，检查一下上面两层循环有无忘记减1的。
    // ERROR:Run-Time Check Failure #2 - Stack around the variable 'testArray' was corrupted.
}
```
### 2.Selction Sort

**一句话总结：**选择排序。第一次找最小的数，第二次找第二小的数，以此类推。

整一个临时的minIndex变量用来记位置。

第一轮：从第**0**个数开始挨个和自己下面的数比较，每次比自己小的那个数字，就把他更新到你这个minIndex里。最后比完你这个minIndex里存的就是最小的数的位置，你这个**0**号位就和这个minIndex号位交换。

第二轮：从第**1**个数开始挨个和自己下面的比较，然后%￥#@（同上），你这个**1**号位就和这个minIndex号位交换。

```c++
void SelctionSort(){
	printf("Selction Sort:\n");
	int testArray[] = {66, 8, 24, 12, 39, 48, 57};
	int arrLength = sizeof(testArray)/sizeof(testArray[0]);

	for(int i=0; i < (arrLength - 1); i++){
		int minIndex = i;
		for(int j=i+1; j < arrLength; j++){
			if(testArray[minIndex] > testArray[j]){
				minIndex = j;
			}
		}
        
		if(i != minIndex){
			testArray[i] ^= testArray[minIndex];
			testArray[minIndex] ^= testArray[i];
			testArray[i] ^= testArray[minIndex];
		}
	}

	// output
	for(int i=0; i < arrLength; i++){
		printf("%d\n",testArray[i]);
	}
}
```

### 3.Insertion Sort

**一句话总结：**插入排序。假定前n-1个数已经排好序，现在将第n个数插到前面的有序数列里。

和自己前面的数组从前一个开始一直比到第0个，碰到比自己大的就插队到他前面（也就换个位置，毕竟从后往前是从大到小）。

第一轮：第0个数就是第1个数前面已经排好序的数组（就一个数哪有什么顺序）。第1个数和第0个数比较，比0号小就互相交换，比0大就不做操作。

第二轮：第0个数和第1个数已经排好序的数组（就一个数哪有什么顺序）。第2个数依次和第0个数，第1个数比较，比n小就和n交换，比两个都大大就不做操作。

```c++
void InsertionSort(){
	printf("Insertion Sort:\n");
	int testArray[] = {66, 8, 24, 12, 39, 48, 57};
	int arrLength = sizeof(testArray)/sizeof(testArray[0]);

    for(int i=0; i < (arrLength - 1); i++){
		for(int j=i+1; j > 0; j--){
			if(testArray[j-1] > testArray[j]){
				testArray[j-1] ^= testArray[j];
				testArray[j] ^= testArray[j-1];
				testArray[j-1] ^= testArray[j];
			} else {
				break;
			}
		}
	}

	// output
	for(int i=0; i < arrLength; i++){
		printf("%d\n",testArray[i]);
	}
```
### 4.Shell Sort

**一句话总结：**希尔排序（缩小增量排序）。

是个不稳定的排序。

改进的插入排序。基本思路是先将整个数据序列分割成若干子序列分别进行直接插入排序，待整个序列中的记录基本有序时，再对全部数据进行依次直接插入排序。步长的选择是希尔排序的关键，只要最终步长为1，任何步长序列都可以。建议最初步长选择为数据长度的一半，直到最终的步长为1。大致就是选定一个步长（每组的数量），分别做插入排序。然后逐渐缩小不长直到变成1（1就是插入排序了，插入排序在对几乎已经排好序的数据操作时，效果非常好）。

我吐了，网上大家写的希尔都8一样的。

我的是这样的

本组的Index=第一个index值+步长*N(N=0~..，看循环次数啦)

第一轮，每组第0个数在本组进行插入排序。

第二轮，每组第1个数在本组进行插入排序。

具体插入排序请看上面插入排序的流程。

**我的垃圾代码比别人长好多.jpg**

```C++
void ShellSort() {
    printf("Shell Sort:\n");
    int testArray[] = { 66, 8, 24, 12, 39, 48, 57 , 77, 88, 90};
    int arrLength = sizeof(testArray) / sizeof(testArray[0]);//等于10

    int incre = arrLength / 2;//选定初始步长（增长量），这里初始步长为5

    while (incre >= 1) {
        for (int i = 0; i < (arrLength - incre);i += incre) {
            for (int j = i + incre; j > 0; j -= incre) {
                for (int k = 0; k < incre; k++) {
                    if (testArray[j + k  - incre] > testArray[j + k]) {
                        testArray[j + k - incre] ^= testArray[j + k];
                        testArray[j + k] ^= testArray[j + k - incre];
                        testArray[j + k - incre] ^= testArray[j + k];
                    }
                    else {
                        break;
                    }
                }
            }
        }
        if (incre == 1) {
            incre = 0;
        }
        else {
            incre = incre / 2;
        }
    }

    // 结果输出
    for (int i = 0; i < arrLength; i++) {
        printf("%d\n", testArray[i]);
    }

}
```


看到个教程，最外层是步长0~步长（我的是步长在最内圈）

总结起来就是，第一组先插入排序然后第二组再插入排序。

我的是每次一起排，按Index计循环。~~我特么开始怀疑我的是不是别的排序算法了。~~

随着学习也许会逐渐明白吧。



### 5.Quick Sort

**一句话总结：**快速排序，分治，选个基数，小的扔左大的扔右。

这是个很高效很重要的排序，所以这一篇讲得也会多一点。其思想很重要。

可爱又特别号懂的的小视频：

[What's the fastest way to alphabetize your bookshelf? - Chand John（油管中字）](https://www.youtube.com/watch?v=WaNLJf8xzC4)

和上面一样内容的备用链

[【TED-ed】快速排序是什么（B站中字）](https://b23.tv/B59ZUH)

具体实施，关于挖坑的生动讲解：

[菜鸟里很好的解释了“挖坑”。](https://www.runoob.com/w3cnote/quick-sort.html)

基准数的选取没有规定。有选第一个的有选中间的有选随机的。

就是先选一个位置的数字当作基准数，我这里就选index为0时.

用一个临时变量int tmp=A[0]；A[0]的值存进tmp之后，A[0]里算是可以放别的值了（就是所谓挖坑）。

例：

现在我有个数组长度为8，

一个索引i=0,一个索引j=8，基本数开头那个数字A[0]假设是4

tmp把A[0]接出来

j从最后开始找，假设找到A[7]，是等于3，比4小，好的去左边，A[7]这个3填进A[0]的位置，现在A[7]位置虽然还是3但是以及没用了，等下一个来填充。

i从最前开始找，假设找到A[3]，等于5，比4大，好的去右边，这个右边不就是A[7]刚好空出来的嘛！就把A[3]的值给A[7]了！

所以我们会发现，我们还需要一个数来记录index,我们就叫他tmpIndex（8过实际代码中并不需要她）了！

当i=j时，他们就会师了，tmp的值就给那个tmpIndex位置的数就好辣。

上面是一份分左右两边,两组。

现在两份分左右！四组。

然后4份分左右！八组每组只有一个数了，就基本上完成了。两两换位置而已嘛。

递归法

```C++
void quickSort() {
    int testArray[] = { 66, 8, 24, 12, 39, 48, 57 , 77};
    int arrLength = sizeof(testArray) / sizeof(testArray[0]);
    quickSort_recursion(testArray, 0, arrLength-1);//开始，最左和最右

    // 结果输出
    for (int i = 0; i < arrLength; i++) {
        printf("%d\n", testArray[i]);
    }
}

void quickSort_recursion(int testArray[], int left, int right)
{
    // i->
    // <-j
    if (left < right)
    {
        int i = left, j = right, x = testArray[left];
        while (i < j)
        {
            // 从右向左找第一个小于基准数x的数
            while (i < j && testArray[j] >= x) j--;//掠过这些比基本数大的数，大的本就该呆在右边 
            if (i < j) testArray[i++] = testArray[j];

            // 从左向右找第一个大于等于基准数x的数
            while (i < j && testArray[i] < x)  i++;//掠过这些比基本数小的数，大的本就该呆在左边 
            if (i < j) testArray[j--] = testArray[i];
        }
        testArray[i] = x;
        quickSort_recursion(testArray, left, i - 1); // 递归调用 
        quickSort_recursion(testArray, i + 1, right);
    }
}
```
**重要**：虽然说最后分到每组只有一个数的时候就是完成了，其实一般分到足够小的时候可以考虑用其他算法。





### 6.Bin Sort

**一句话总结:**自己的值就是index。

关于一句话总结，某些网站就是这么教的，虽然差不多但是也不至于这么简单粗暴，在考试的时候一下子想不起来怎么搞的话这么搞也不错哈哈哈。

先整个暴力版，java倒是可以直接.add进新数组，嘛，C++也能链表嘛，不过都说是暴力版了。。有空写优雅来~~（不会有空~~

```C++
void BinSort_Simple() {
    printf("Bin Sort:\n");
    int testArray[] = { 66, 8, 24, 12, 39, 48, 57 };
    int arrLength = sizeof(testArray) / sizeof(testArray[0]);

    int newArr[100] = {};
    //memset(newArr, NULL, sizeof(newArr[100]));//另一种初始化方法惹

    for (int i = 0; i < arrLength;i++) {
        int tmp  = testArray[i];
        newArr[tmp] = tmp;
    }

    // 结果输出
    for (int i = 0; i < 100; i++) {
        if (newArr[i] != 0) {
            printf("%d\n", newArr[i]);
        }
    
    }
}
```
前面说的都是**内部排序**，而这个呢，是**外部排序**哦。

箱排序也称桶排序(Bucket Sort)

优化算法是基数排序(RadixSort)，这个再之后会写到。



### 7.Heap Sort

**一句话总结：**没有一句话总结，就是堆排序。

不明白堆的特性和公式可以看看这个

https://www.jianshu.com/p/6b526aa481b1

俺们需要知道（堆基础姿势）

i 是节点的索引

parent(i) = floor((i - 1)/2)
left(i)   = 2i + 1
right(i)  = 2i + 2

[十大排序算法----堆排序（最后一个非叶子节点的序号是n/2-1的推理）](https://blog.csdn.net/qq_45844792/article/details/106026262)

开始前，需要构建初始堆，关于堆的特性，不是很了解的可以[百度一下：堆](https://baike.baidu.com/item/%E5%A0%86/20606834?fr=aladdin)

初期构建一个大顶堆（其实已经是第一轮了）然后下一轮。

每一轮都会找出最大的那个数字放，然后把他扔到最后面去，接着就是length-1个数找出最大，再放到倒数第二个。

数组上看来是如此，不过再大多数的形容中我们可以理解为，每轮堆的长度减1。

具体就是堆尾开始和兄弟比较，然后两者取大的那个和自己的parent比较。比到最后就是最大的在堆首了，然后和堆尾交换这样就去最后一个了，然后堆就减掉一个长度（就是找出来了的这个数），继续下一轮。

```C++
void Max_heapify(int arr[], int start, int end) {
    // 建立父節點指標和子節點指標
    int dad = start;
    int son = dad * 2 + 1;
    while (son <= end) { // 若子節點指標在範圍內才做比較
        if (son + 1 <= end && arr[son] < arr[son + 1]) // 先比較兩個子節點大小，選擇最大的
            son++;
        if (arr[dad] > arr[son]) // 如果父節點大於子節點代表調整完畢，直接跳出函數
            return;
        else { // 否則交換父子內容再繼續子節點和孫節點比較
            arr[dad] ^= arr[son];
            arr[son] ^= arr[dad];
            arr[dad] ^= arr[son];

            dad = son;
            son = dad * 2 + 1;
        }
    }
}

void HeapSort() {
    printf("Heap Sort:\n");
    int testArray[] = { 66, 8, 24, 12, 39, 48, 57 };
    int arrLength = sizeof(testArray) / sizeof(testArray[0]);

    // 构建初始大根堆，i從最後一個父節點開始調整
    for (int i = arrLength / 2 - 1; i >= 0; i--)//(arrLength / 2 - 1)是最后一个非叶子节点（最后一个父节点）
        Max_heapify(testArray, i, arrLength - 1);

    // 先將第一個元素和已经排好的元素前一位做交換，再從新調整(刚调整的元素之前的元素)，直到排序完畢
    for (int i = arrLength - 1; i > 0; i--) {//每轮堆数长-1
        //堆首和堆尾交换
        testArray[0] ^= testArray[i];
        testArray[i] ^= testArray[0];
        testArray[0] ^= testArray[i];
        
        // 重新构建大根堆
        Max_heapify(testArray, 0, i - 1);
    }

    // 结果输出
    for (int i = 0; i < arrLength; i++) {
        printf("%d\n", testArray[i]);
    }
}
```

参考：

[堆排序算法（图解详细流程）](https://blog.csdn.net/u010452388/article/details/81283998)

*https://github.com/hustcc/JS-Sorting-Algorithm/blob/master/7.heapSort.md*



### 8.RadixSort

基数排序

![](https://www.runoob.com/wp-content/uploads/2019/03/radixSort.gif)

## REFER

十大经典算法：https://zhuanlan.zhihu.com/p/41923298

内部排序和外部排序小结https://blog.csdn.net/qq_36189382/article/details/81668098

有趣的视频：舞动的算法

选择排序：[av17004884](https://www.bilibili.com/video/av17004884/)
冒泡排序：[av17004846](https://www.bilibili.com/video/av17004846/)
快速排序：[av17004952](https://www.bilibili.com/video/av17004952/)
插入排序：[av17004913](https://www.bilibili.com/video/av17004913/)
希尔排序：[av17004970](https://www.bilibili.com/video/av17004970/)
归并排序：[av17004931](https://www.bilibili.com/video/av17004931/)
堆排序：[av32877402](https://www.bilibili.com/video/av32877402/)

堆排序[BV11W411m](https://www.bilibili.com/video/BV11W411m7Zc)



##  OTHER

video(bilibili):https://www.bilibili.com/video/av838535161

video（youtube）:https://www.youtube.com/watch?v=LOZTuMds3LM

github:https://github.com/MusicTheorist/ArrayVisualizer

WurstRELOADED

Exchange family

0:04 Bubble Sort

0:22 Cocktail Shaker Sort

0:37 Gnome Sort

1:05 Optimized Gnome Sort

1:22 Odd-Even Sort

1:41 Comb Sort

2:09 Quick Sort

2:22 Stable Quick Sort

2:43 Dual-Pivot Quick Sort



Selection family

3:05 Selection Sort

3:16 Double Selection Sort

3:28 Max Heap Sort

3:52 Min Heap Sort

4:17 Ternary Heap Sort

4:37 Weak Heap Sort

4:53 Smooth Sort

6:07 Tournament Sort



Insertion family

6:37 Insertion Sort

6:54 Binary Insertion Sort

7:12 Shell Sort

5:15 Cycle Sort

7:28 Patience Sort



Merge family

7:41 Merge Sort

8:30 In-Place Merge Sort



Non-comparison family

9:00 American Flag Sort, 256 Buckets

9:14 Bead (Gravity) Sort

9:40 Counting Sort

9:46 Pidgeonhole Sort

9:52 Radix LSD Sort, Base 4

10:22 In-Place Radix LSD Sort, Base 16

10:46 Radix MSD Sort, Base 8

11:08 Flash Sort

11:20 Shatter Sort

11:39 Simple Shatter Sort

12:00 Time Sort (Mul 4) [more widely known as Sleep Sort]



robinbzSorting Networks / Concurrent family

12:16 Batcher's Bitonic Sort

13:11 Batcher's Odd-Even Merge Sort



Hybrids

13:34 Hybrid Comb Sort (Comb/Insertion)

13:55 Binary Merge Sort (Merge/Binary Insertion)

14:41 Weave Mergee Sort (Merge/Insertion)

15:34 TimSort

16:18 WikiSort (Block Merge Sort)

16:57 GrailSort (Block Merge Sort)

17:24 std::sort (Introsort)

17:46 Quick Shell Sort (Introsort with Shellsort)

18:06 std::stable_sort (Insert/Bottom-up Merge)



Esoteric / Fun

18:35 Pancake Sort

21:14 Stooge Sort

22:09 Bad Sort 

22:30 Silly Sort

24:10 Slow Sort

25:51 Less Bogo Sort

27:10 Cocktail Bogo Sort

28:37 Bogo Sort





