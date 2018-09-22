---
title: 算法导论day1
date: 2018-01-26 02:50:20
tags: [排序,分治]
categories: [算法导论blog]
---
## 想要撸一遍算法导论的想法很早就有的，但是人之懒性无奈之，直到看到一句励志的话，你永远不知道，你以后要有多努力，才能弥补现在的懒惰。我这人很懒，索性现在稍微努力点，也是为了以后可以偷懒。所以now rather thinking than action。共勉之...
## 算法导论day1
### 算法在计算中的作用
算法(algorithm)就是任何良定义的计算过程，该过程取某个值或值的集合作为输入并产生某个值或值的集合作为输出。这样算法就是把输入转换成输出的计算步骤的一个序列。
### 算法基础
#### 插入排序
对于少量元素的排序，它是一个有效的算法。工作方式类似于排序一手扑克牌，开始时，我们的左手为空并依次拿走桌上的一张牌插入左手中正确的位置。为了找到一张牌的正确位置，我们从右到左将它与已在手中的每张牌进行比较。拿在手中的牌总是排序好的。
``` java
package com.JackeyZz.day1;
public class sort {
	public static void main(String[] args) {
		int n=(int) (Math.random()*10+1);
		int[] arr=new int[n];
		System.out.println("产生的"+n+"个随机数如下：");
		for(int i=0;i<n;i++){
			arr[i]=(int) (Math.random()*100);
			System.out.print(arr[i]+" ");
		}
		System.out.println("插入排序结果如下：");
		insertsort(arr);
		for(int i=0;i<arr.length;i++){
			System.out.print(arr[i]+" ");
		}
	}
	public static void insertsort(int[] arr){
		int key;
		for(int j=1;j<arr.length;j++){
			key=arr[j];
			int i=j-1;
			while(i>=0&&arr[i]>key){
				arr[i+1]=arr[i];
				i=i-1;
			}
			arr[i+1]=key;
		}
	}
}
```
#### 分析算法
一般来说，算法所需要的时间与输入的规模同步增长，所以通常把一个程序的运行时间描述成其输入规模的函数。  
输入规模的最佳概念依赖于研究的问题.如排序或者计算离散傅里叶变换，最自然的量度是输入中的项数。  
一个算法在特定输入上的运行时间是指执行的基本操作数或步数。
#### 设计算法
1、分治法  
许多有用的算法在结构上是递归的：为了解决一个给定的问题，算法一次或者多次递归地调用其自身以解决紧密相关的若干子问题。这些算法典型地遵循分治法的思想：将原问题分解为几个规模较小但类似于原问题的子问题，递归地求解这些子问题，然后再合并这些子问题的解来建立原问题的解。  
**归并排序算法**完全遵循了分治模式，直观上的操作如下：  
**分解**：分解待排序的n个元素的序列成各具n/2个元素的两个子序列。  
**解决**：使用归并排序递归地排序两个子序列。  
**合并**：合并两个已排序的子序列以产生已排序的答案。
``` java
package com.JackeyZz.day1;
public class sort {
	public static void main(String[] args) {
		int n=(int) (Math.random()*10+1);
		int[] arr=new int[n];
		System.out.println("产生的"+n+"个随机数如下：");
		for(int i=0;i<n;i++){
			arr[i]=(int) (Math.random()*100);
			System.out.print(arr[i]+" ");
		}
		System.out.println("插入排序结果如下：");
		//insertsort(arr);
		//int[] arr1={3,1,2,4,5,6};
		//System.out.println(arr1.length);
		//merge(arr,0,1,arr.length-1);
		mergesort(arr,0,arr.length-1);
		for(int i=0;i<arr.length;i++){
			System.out.print(arr[i]+" ");
		}
	}
	public static void merge(int[] arr,int p,int q,int r){
		int n1,n2;
		int i,j;
		n1=q-p+1;
		//System.out.println(n1);
		n2=r-q;
		int[] L=new int[n1];
		int[] R=new int[n2];
		for(i=0;i<n1;i++){
			L[i]=arr[p+i];
			//System.out.print(L[i]+" ");
		}
		//System.out.println("");
		for(j=0;j<n2;j++){
			R[j]=arr[q+j+1];
			//System.out.print(R[j]+" ");
		}
		//System.out.println("");
		//L[n1]=(int) Double.NEGATIVE_INFINITY;
		//R[n2]=(int) Double.NEGATIVE_INFINITY;
		for(int x=0,y=0,k=p;k<r;k++){
			if(L[x]<R[y]){
				arr[k]=L[x];
				x=x+1;
			}
			else{
				arr[k]=R[y];
				y=y+1;
			}
			if(x>=n1){
				System.arraycopy(R, y, arr, k+1, R.length-y);
				break;
			}
			if(y>=n2){
				System.arraycopy(L, x, arr, k+1, L.length-x);
				break;
			}
		}
	}
	public static void mergesort(int[] arr,int p,int r){
		if(p<r){
			int q=(int) Math.floor((p+r)/2);
			mergesort(arr,p,q);
			mergesort(arr,q+1,r);
			merge(arr,p,q,r);
		}
	}
}
```
归并排序算法的关键操作在于“合并”步骤中两个已排序序列的合并。合并merge的过程用扑克牌解释：两堆已排好序的牌，最小的牌在最上面，比较最上面的两张牌，将较小的牌从堆上移出，重复这个步骤，直至其中一堆为空，再将剩余的堆直接移到输出堆。   
在递归中返回条件是p≥r，即该子数组最多有一个元素，所以是已经排好序的，再由递归一步一步返回，直至返回排好序的序列。
```
graph TD
    A1[5] -->|合并|B1(2 5)
    A2[2] -->|合并|B1
    A3[4] -->|合并|B2(4 7)
    A4[7] -->|合并|B2
    A5[1] -->|合并|B3(1 3)
    A6[3] -->|合并|B3
    A7[2] -->|合并|B4(2 6)
    A8[6] -->|合并|B4
    B1 -->|合并|C1(2 4 5 7)
    B2 -->|合并|C1(2 4 5 7)
    B3 -->|合并|C2(1 2 3 6)
    B4 -->|合并|C2(1 2 3 6)
    C1 -->|合并|D(1 2 2 3 4 5 6 7)
    C2 -->|合并|D(1 2 2 3 4 5 6 7)
```
2、分析分治算法  
分治算法运行时间的递归式来自基本模式的三个步骤。假设T(n)是规模为n的一个问题的运行时间，将原问题分解为a个子问题，每个子问题的规模是原问题的1/b(对于归并排序，a和b都为2),为了求解一个规模为n/b的子问题，需要T(n/b)的时间，所以需要aT(n/b)的时间来求解a个子问题。如果分解问题为子问题需要时间D(n)，合并子问题的解成原问题的解需要时间C(n)，则得到

3、冒泡排序  
冒泡排序是一种简单的排序算法，名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。  
算法flash演示：  
[come in link](http://student.zjzk.cn/course_ware/data_structure/web/flashhtml/maopaopaixu.htm)  
``` java
package com.JackeyZz.day1;

public class sort {
	public static void main(String[] args) {
		int n=(int) (Math.random()*10+1);
		int[] arr=new int[n];
		System.out.println("产生的"+n+"个随机数如下：");
		for(int i=0;i<n;i++){
			arr[i]=(int) (Math.random()*100);
			System.out.print(arr[i]+" ");
		}
		System.out.println("插入排序结果如下：");
		//insertsort(arr);
		//mergesort(arr,0,arr.length-1);
		bubblesort(arr);
		for(int i=0;i<arr.length;i++){
			System.out.print(arr[i]+" ");
		}
	}
	public static void bubblesort(int[] arr){
		int key;
		for(int i=0;i<arr.length-1;i++){
			for(int j=arr.length-1;j>i;j--){
				if(arr[j]<arr[j-1]){
					key=arr[j];
					arr[j]=arr[j-1];
					arr[j-1]=key;
				}
			}
		}
	}
}
```