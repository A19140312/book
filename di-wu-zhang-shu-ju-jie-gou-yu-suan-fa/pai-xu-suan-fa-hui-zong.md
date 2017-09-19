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


```java


```





