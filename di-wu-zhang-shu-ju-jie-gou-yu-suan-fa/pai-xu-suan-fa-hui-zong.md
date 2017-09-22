#排序算法汇总
各种排序算法的时间复杂度以及稳定性
![](http://upload-images.jianshu.io/upload_images/273973-19cf4a1e58b6ebaf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##冒泡排序
*    算法原理：相邻的数据进行两两比较，小(大)数放在前面，大(小)数放在后面，这样一趟下来，最小(大)的数就被排在了第一位，第二趟也是如此，如此类推，直到所有的数据排序完成。
*    时间复杂度：**最坏：O(n2) 最好: O(n)** 平均: O(n2) 空间复杂度：O(1) 稳定性：稳定。


```java
/**
 * 冒泡排序（升序排列）
 * @param array
 */
public void bubbleSort(int[] a){
	int len = a.length;
	for(int i = 0 ; i < len ; i ++){
		for(int j = 1 ; j < len ; j ++){
			if(a[j-1] > a[j]){
				int temp = a[j-1];
				a[j-1] = a[j];
				a[j] = temp;
			}
		}
	}
}
```
##选择排序
*	算法原理：

	先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。
*	时间复杂度：**最坏：O(n2) 最好: O(n)** 平均: O(n2) 空间复杂度：O(1) 稳定性： 稳定 



```java
/**
 * 选择排序（升序排列）
 * @param array
 */

public void selectSort(int[] a){
	int len = a.length;
	for(int i = 0 ; i < len ; i ++){
		int flag = i;
		for(int j = i+1 ; j < len ; j ++){
			if(a[j] < a[flag]){
				flag = j;
			}
		}
		if(i != flag){
			int temp = a[i];
			a[i] = a[flag];
			a[flag] = temp;
		}
	}
}

```
##直接插入排序
*	算法原理：
每次将一个待排序的数据按照其关键字的大小插入到前面已经排序好的数据中的适当位置，直到全部数据排序完成。
*	时间复杂度：**最坏：O(n2) 最好: O(n)** 平均: O(n2) 空间复杂度：O(1) 稳定性： 稳定 

*	排序演示：![](http://wuchong.me/img/Insertion-sort-example-300px.gif)

```java
/**
 * 直接插入排序（升序排列）
 * @param array
 */

public void insertSort(int[] a){
	int len = a.length;
	for(int i = 1 ; i < len ; i ++){
		 int temp = a[i],j;
		for(j = i ; j > 0 && a[j-1] > temp; j --){
			a[j] = a[j-1];
		}
		a[j] = temp;
	}
}

```

##快速排序
*	算法原理：
	*	从数列中挑出一个元素作为基准数。
	*	分区过程，将比基准数大的放到右边，小于或等于它的数都放到左边。
	*	再对左右区间递归执行第二步，直至各区间只有一个数。
*	时间复杂度：**最坏：O(n2) 最好: O(nlogn)** 平均: O(nlogn) 空间复杂度：O(logn) 稳定性： **不稳定 **

*	排序演示：![](http://wuchong.me/img/Quicksort-example.gif)

```java
/**
 * 快速排序-固定基准点（升序排列）
 * @param array
 */

public void quickSort(int[] a,int l, int r){
	if(l >= r) return;
	int flag = l;
	int i = l , j = r;
	while(i < j){
		while(i < j && a[j] > a[flag])  j -- ;
		while(i < j && a[i] < a[flag])  i ++ ;
		if(i != j){
			int temp = a[i];
			a[i] = a[j];
			a[j] = temp;
		}
	}
	if(i != flag){
		int temp = a[i];
		a[i] = a[flag];
		a[flag] = temp;
	}
	quickSort(a, l, i-1);
	quickSort(a, i+1, r);
}

```
快排的最差情况为序列完全有序，此时快排退化为冒泡排序，时间复杂度为 O(n2)。
*	**快排的优化**
	* 随机选取基准



