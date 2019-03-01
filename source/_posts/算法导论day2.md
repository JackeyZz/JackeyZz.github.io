---
title: 算法导论day2
date: 2018-01-26 02:50:27
tags: [函数增长的符号,最大子数组问题,矩阵乘法]
categories: [算法导论blog]
---
## 最糟糕的是人们在生活中经常受到错误志向的阻碍而不自知，真到摆脱了那些阻碍时才能明白过来。万事开头难，希望能够persistence。  
## 算法导论day2
### 函数的增长
#### 渐近精确界记号：Θ(big-theta)
对于算法的运行时间表达式`$T(n)=30n^4+20n^3+40n^2+46n+100$`，当问题规模足够大的时候如n=100万，则算法的运行时间主要取决于表达式的第一项，于是，运行时间可以记为：`$T(n) \approx n^4$`，即`$T(n)= \theta(n^4)$`。  
> `$\theta$`的数学含义在于：对于所有的`$n>n_0$`时，函数f(n)乘上一个常量因子可等于g(n)，则称g(n)是f(n)的一个渐进精确界。g(n)即可以表示上界也可以表示下界。
#### 渐近上界记号：`$O(big-oh)$`
> 定义：设f(n)和g(n)是定义域为自然数集N上的函数。若存在正数c和`$n_0$`，使得对一切`$n \geq n_0$`都有0≤f(n)≤cg(n)成立，则称f(n)的渐进的上界是g(n)，记作f(n)=O(g(n))。通俗的说n满足一定条件范围内，函数f(n)的阶不高于函数g(n)。
#### 渐近下界记号：`$\Omega(big-omega)$`
> 定义：设f(n)和g(n)是定义域为自然数集N上的函数。若存在正数c和`$n_0$`，使得对一切`$n \geq n_0$`都有0≤cg(n)≤f(n)成立，则称f(n)的渐进的下界是g(n)，记作f(n)=Ω(g(n))。通俗的说n满足一定条件范围内，函数f(n)的阶不低于函数g(n)。
#### 非渐近精确上界：o(small-oh)
> 定义1：设f(n)和g(n)是定义域为自然数集N上的函数。若对于任意正数c，都存在`$n_0$`，使得对一切`$n \geq n_0$`都有0≤f(n)<cg(n)成立，则称f(n)的渐进的非紧确上界是g(n)，记作f(n)=o(g(n))。通俗的说n满足一定条件范围内，函数f(n)的阶低于函数g(n)。  
> 定义2：设f(n)和g(n)是定义域为自然数集合的函数。如果`$\lim_{x\to \infty} \frac{f(n)}{g(n)}=0$`，那么f(n)=o(g(n))。通俗理解为f(n)低于g(n)的阶。
#### 非渐近精确下界：`$\omega(small-omega)$`
> 定义1：设f(n)和g(n)是定义域为自然数集N上的函数。若对于任意正数c，都存在`$n_0$`，使得对一切`$n \geq n_0$`都有0≤cg(n)<f(n)成立，则称f(n)的渐进的非紧确下界是g(n)，记作f(n)=ω(g(n))。通俗的说n满足一定条件范围内，函数f(n)的阶高于函数g(n)。   
> 定义2：设f(n)和g(n)是定义域为自然数集合的函数。如果`$\lim_{x\to \infty} \frac{f(n)}{g(n)}=\infty$`，那么f(n)=o(g(n))。通俗理解为f(n)高于g(n)的阶。
#### 可以参考下面的资料
[渐近记号详解](http://blog.csdn.net/so_geili/article/details/53353593#comments)  
[习题答案](https://www.cnblogs.com/timebug/archive/2010/03/25/1694286.html)
#### 总结
- Θ(g(n))={ f(n): 存在正常数c1,c2和n0，使对所有的n>=n0，有0<=c1g(n)<=f(n)<=c2g(n) }
- O(g(n))={ f(n): 存在正常数c和n0，使对所有n>=n0，有0<=f(n)<=cg(n) }
- Ω(g(n))={ f(n): 存在正常数c和n0，使对所有n>=n0，有0<=cg(n)<=f(n) }
- o(g(n))={ f(n): 对任意正常数c，存在常数n0>0，使对所有的n>=n0，有0<=f(n)<=cg(n) }
- ω(g(n))={ f(n): 对任意正常数c，存在常数n0>0，使对所有的n>=n0，有0<=cg(n)<f(n) }  
例子：对于O来讲，它是用来表示上界的，当用它作为算法的最坏情况运行时间的上界时，就有对任意输入的运行时间的上界。我们说“一个算法A的运行时间为`$O(n^2)$`”，它表示的是说该算法运行时间的一个上界，适用于每个输入的运行时间。
### 分治策略
#### 最大子数组问题  
寻找数组A的和最大的非空连续子数组的问题。只有当数组中包含负数时，最大子数组问题才有意义。如果所有的数组元素都是非负的，最大子数组问题没有任何难度，因为整个数组的和肯定是最大的。
```java
package com.JackeyZz.day2;
class Result{
	int low;
	int high;
	int sum;
}
public class divide {
	public static void main(String[] args) {
		int n=(int) (Math.random()*10+1);
		int[] arr=new int[n];
		System.out.println("产生的"+n+"个随机数如下：");
		for(int i=0;i<n;i++){
			int x=(int) (Math.random()*10+1);
			arr[i]=(int) ((int) (Math.random()*100)*Math.pow(-1, x));
			System.out.print(arr[i]+" ");
		}
		System.out.println("");
		System.out.println("输出的最大子数组为：");
		//int[] arr1={2,-1,8,9,-5,10,5,1,-8,4,-5,-6,-3,7};
		//int[] arr1={5,-7,3,4,5,6,8,-10,-12};
		Result result=new Result();
		//result=find_max_crossing_subarray(arr1, 0, 6, arr1.length-1);
		result=find_maximum_subarray(arr, 0, arr.length-1);
		System.out.println(result.low+" "+result.high+" "+result.sum);
	}
	public static Result find_max_crossing_subarray(int[] arr,
			int low,int mid,int high){
		Result result = new Result();
		int left_sum=(int) Double.NEGATIVE_INFINITY;
		int right_sum=(int) Double.NEGATIVE_INFINITY;
		int sum = 0;
		for(int i=mid;i>=low;i--){
			sum=sum+arr[i];
			if(sum>left_sum){
				left_sum=sum;
				result.low=i;
			}
		}
		sum=0;
		for(int j=mid+1;j<=high;j++){
			sum=sum+arr[j];
			if(sum>right_sum){
				right_sum=sum;
				result.high=j;
			}
		}
		result.sum=left_sum+right_sum;
		return result;
	}
	public static Result find_maximum_subarray(int[] arr,int low,int high){
		int mid;
		Result left_result = new Result();
		Result right_result = new Result();
		Result cross_result = new Result();
		if(high==low){
			left_result.low=low;
			left_result.high=low;
			left_result.sum=arr[low];
			return left_result;
		}
		else{
			mid=(low+high)/2;
			left_result=find_maximum_subarray(arr, low, mid);
			right_result=find_maximum_subarray(arr, mid+1, high);
			cross_result=find_max_crossing_subarray(arr, low, mid, high);
			if(left_result.sum>=right_result.sum 
					&& left_result.sum>=cross_result.sum){
				return left_result;
			}
			else if(right_result.sum>=left_result.sum 
					&& right_result.sum>=cross_result.sum){
				return right_result;
			}
			else{
				return cross_result;
			}
		}
	}
}
```  
很容易就知道数组A的任何连续子数组A[i..j]必然处于以下三种情况：  
1. 完全处于子数组A[low..mid]中
2. 完全处于子数组A[mid+1..high]中
3. 跨越了中点mid  
因此只要求出跨越中点mid的最大连续子数组的值，另外的左右子数组递归，最后比较三者的大小，即可得出最后的最大连续子数组。  
- 递归流程  
程序先执行left递归，直至满足low==high,返回left_result，接着执行right，立即返回right_result，可能会执行cross，返回cross_result,最后比较三者大小，返回最大值。(这是left递归最底层的执行过程)接着返回到left的上一层递归，此时的left_result就是刚刚比较的最大值，紧接着执行这一层的right，此时需要执行right递归，最后返回这一层的right_result(同样是比较的最大值)，可能也要执行cross返回cross_result,最后再比较三者大小并返回left的再上一层递归。如此循环，最后返回的就是整个数组的最大连续子数组。  
#### 矩阵乘法的Strassen算法 
矩阵相乘这一部分算法导论介绍了三个解决方法，分别是暴力法，直接递归分治算法以及Strassen算法。  
下面的代码片段是前两种方法的实现。递归分治算法的难点主要在于构建子矩阵以及合成矩阵这两方面。
```java
package com.JackeyZz.day2;
public class divide1 {
	public static void main(String[] args) {
		int[][] A=new int[4][4];
		int[][] B=new int[4][4];
		System.out.println("产生的8*8矩阵A：");
		for(int i=0;i<A.length;i++){
			for(int j=0;j<A[0].length;j++){
				A[i][j]=(int) (Math.random()*10+1);
				System.out.print(A[i][j]+" ");
			}
			System.out.println("");
		}
		System.out.println("产生的8*8矩阵B：");
		for(int i=0;i<B.length;i++){
			for(int j=0;j<B[0].length;j++){
				B[i][j]=(int) (Math.random()*10+1);
				System.out.print(B[i][j]+" ");
			}
			System.out.println("");
		}
		System.out.println("暴力法矩阵相乘结果：");
		int[][] result1=new int[4][4];
		result1=Matrix_multiply(A, B, 4);
		for(int i=0;i<4;i++){
			for(int j=0;j<4;j++){
				System.out.print(result1[i][j]+" ");
			}
			System.out.println("");
		}
		System.out.println("分治法矩阵相乘结果：");
		int[][] result=null;
		result=square_matrix_multiply_recursive(A, B, 4);
		for(int i=0;i<result.length;i++){
			for(int j=0;j<result[0].length;j++){
				System.out.print(result[i][j]+" ");
			}
			System.out.println("");
		}
	}
	//暴力求解矩阵相乘
	public static int[][] Matrix_multiply(int[][] A,int[][] B,int n){
		int[][] result=new int[n][n];
		for(int i=0;i<n;i++){
			for(int j=0;j<n;j++){
				result[i][j]=0;
				for(int k=0;k<n;k++){
					result[i][j]+=A[i][k]*B[k][j];
				}
			}
		}
		return result;
	}
	//递归求解矩阵乘法
	public static int[][] square_matrix_multiply_recursive(int[][] A,
			int[][] B,int n){
		int[][] result=new int[n][n];
		if(n==2){
			result=Matrix_multiply(A, B, n);
			return result;
		}
		if(n>2){
			int m=n/2;
			int[][] A11=QuarterMatrix(A, n, 1);
			int[][] A12=QuarterMatrix(A, n, 2);
			int[][] A21=QuarterMatrix(A, n, 3);
			int[][] A22=QuarterMatrix(A, n, 4);
			
			int[][] B11=QuarterMatrix(B, n, 1);
			int[][] B12=QuarterMatrix(B, n, 2);
			int[][] B21=QuarterMatrix(B, n, 3);
			int[][] B22=QuarterMatrix(B, n, 4);
			
			int[][] result11=QuarterMatrix(result, n, 1);
			int[][] result12=QuarterMatrix(result, n, 2);
			int[][] result21=QuarterMatrix(result, n, 3);
			int[][] result22=QuarterMatrix(result, n, 4);
			
			//result=square_matrix_multiply_recursive(A11, B11, m);
			result11=AddMatrix(square_matrix_multiply_recursive(A11, B11, m),
					square_matrix_multiply_recursive(A12, B21, m), m);
			result12=AddMatrix(square_matrix_multiply_recursive(A11, B12, m),
					square_matrix_multiply_recursive(A12, B22, m), m);
			result21=AddMatrix(square_matrix_multiply_recursive(A21, B11, m),
					square_matrix_multiply_recursive(A22, B21, m), m);
			result22=AddMatrix(square_matrix_multiply_recursive(A21, B12, m),
					square_matrix_multiply_recursive(A22, B22, m), m);
			result=TogetherMatrix(result11, result12, result21, result22, m);
		}
		return result;
	}
	//获取矩阵的四分之一，并返回子矩阵
	public static int[][] QuarterMatrix(int[][] P,int n,int num){
		int row=n/2;
		int cols=n/2;
		int[][] result=new int[row][cols];
		switch(num){
			case 1:{
				for(int i=0;i<row;i++){
					for(int j=0;j<cols;j++){
						result[i][j]=P[i][j];
					}
				}
				break;
			}
			case 2:{
				for(int i=0;i<row;i++){
					for(int j=0;j<n-cols;j++){
						result[i][j]=P[i][j+cols];
					}
				}
				break;
			}
			case 3:{
				for(int i=0;i<n-row;i++){
					for(int j=0;j<cols;j++){
						result[i][j]=P[i+row][j];
					}
				}
				break;
			}
			case 4:{
				for(int i=0;i<n-row;i++){
					for(int j=0;j<n-cols;j++){
						result[i][j]=P[i+row][j+cols];
					}
				}
				break;
			}
			default:
				break;
		}
		return result;
	}
	//矩阵相加
	public static int[][] AddMatrix(int[][] A,int[][] B,int n){
		int[][] result=new int[n][n];
		for(int i=0;i<n;i++){
			for(int j=0;j<n;j++){
				result[i][j]=A[i][j]+B[i][j];
			}
		}
		return result;
	}
	//整合矩阵
	public static int[][] TogetherMatrix(int[][] a,int[][] b,
			int[][] c,int[][] d,int n){
		int[][] result=new int[2*n][2*n];
		for(int i=0;i<2*n;i++){
			for(int j=0;j<2*n;j++){
				if(i<n){
					if(j<n){
						result[i][j]=a[i][j];
					}
					else{
						result[i][j]=b[i][j-n];
					}
				}
				else{
					if(j<n){
						result[i][j]=c[i-n][j];
					}
					else{
						result[i][j]=d[i-n][j-n];
					}
				}
			}
		}
		return result;
	}
}
```
Strassen算法的核心思想是令递归树稍微不那么茂盛一点，即只递归进行7次而不是8次n/2*n/2矩阵的乘法，因此前两种算法的运行时间是`$O(n^3)$`，而Strassen算法的运行时间是`$O(n^{2.81})$`。  
Strassen算法的公式如下：  
`$S_1=B_{12}-B_{22}$`&emsp;`$S_2=A_{11}+A_{12}$`&emsp;`$S_3=A_{21}+A_{22}$`  
`$S_4=B_{21}-B_{11}$`&emsp;`$S_5=A_{11}+A_{22}$`&emsp;`$S_6=B_{11}+B_{22}$`  
`$S_7=A_{12}-A_{22}$`&emsp;`$S_8=B_{21}+B_{22}$`&emsp;`$S_9=A_{11}-A_{21}$`  
`$S_10=B_{11}+B_{12}$`  
`$P1=A_{11}*S_1$`&emsp;`$P2=S_{2}*B_{22}$`&emsp;`$P3=S_3*B_{11}$`&emsp;`$P4=A_{22}*S_4$`&emsp;`$P5=S_5*S_6$`&emsp;`$P6=S_7*S_8$`&emsp;`$P7=S_9*S_{10}$`&emsp;  
`$C_{11}=P_5+P_4-P_2+P_6$`&emsp;`$C_{12}=P_1+P_2$`&emsp;`$C_{21}=P_3+P_4$`&emsp;`$C_{22}=P_5+P_1-P_3-P_7$`  
实现的代码如下：  
```java
package com.JackeyZz.day2;

public class divide2 {
	public static void main(String[] args) {
		int[][] A=new int[4][4];
		int[][] B=new int[4][4];
		System.out.println("产生随机矩阵A：");
		for(int i=0;i<A.length;i++){
			for(int j=0;j<A[0].length;j++){
				A[i][j]=(int) (Math.random()*10+1);
				System.out.print(A[i][j]+" ");
			}
			System.out.println("");
		}
		System.out.println("产生的随机矩阵B：");
		for(int i=0;i<B.length;i++){
			for(int j=0;j<B[0].length;j++){
				B[i][j]=(int) (Math.random()*10+1);
				System.out.print(B[i][j]+" ");
			}
			System.out.println("");
		}
		
		System.out.println("暴力法矩阵相乘结果：");
		int[][] result1=new int[4][4];
		result1=matrix_multiply(A, B, 4);
		for(int i=0;i<4;i++){
			for(int j=0;j<4;j++){
				System.out.print(result1[i][j]+" ");
			}
			System.out.println("");
		}
		
		System.out.println("Strassen产生的矩阵相乘：");
		int[][] result=null;
		result=strassen_matrix(A, B, 4);
		for(int i=0;i<4;i++){
			for(int j=0;j<4;j++){
				System.out.print(result[i][j]+" ");
			}
			System.out.println("");
		}
	}
	public static int[][] strassen_matrix(int[][] A,int[][] B,int n){
		int[][] result=new int[n][n];
		if(n==2){
			result=matrix_multiply(A, B, n);
			return result;
		}
		if(n>2){
			int m=n/2;
			int[][] S1=null;
			int[][] S2=null;
			int[][] S3=null;
			int[][] S4=null;
			int[][] S5=null;
			int[][] S6=null;
			int[][] S7=null;
			int[][] S8=null;
			int[][] S9=null;
			int[][] S10=null;
			
			int[][] P1=null;
			int[][] P2=null;
			int[][] P3=null;
			int[][] P4=null;
			int[][] P5=null;
			int[][] P6=null;
			int[][] P7=null;
			
			int[][] A11=QuarterMatrix(A, n, 1);
			int[][] A12=QuarterMatrix(A, n, 2);
			int[][] A21=QuarterMatrix(A, n, 3);
			int[][] A22=QuarterMatrix(A, n, 4);
			
			int[][] B11=QuarterMatrix(B, n, 1);
			int[][] B12=QuarterMatrix(B, n, 2);
			int[][] B21=QuarterMatrix(B, n, 3);
			int[][] B22=QuarterMatrix(B, n, 4);
			
			int[][] result1=QuarterMatrix(result, n, 1);
			int[][] result2=QuarterMatrix(result, n, 2);
			int[][] result3=QuarterMatrix(result, n, 3);
			int[][] result4=QuarterMatrix(result, n, 4);
			
			S1=Addmatrix(B12, B22, m, -1);
			S2=Addmatrix(A11, A12, m, 1);
			S3=Addmatrix(A21, A22, m, 1);
			S4=Addmatrix(B21, B11, m, -1);
			S5=Addmatrix(A11, A22, m, 1);
			S6=Addmatrix(B11, B22, m, 1);
			S7=Addmatrix(A12, A22, m, -1);
			S8=Addmatrix(B21, B22, m, 1);
			S9=Addmatrix(A11, A21, m, -1);
			S10=Addmatrix(B11, B12, m, 1);
			
			P1=strassen_matrix(A11, S1, m);
			P2=strassen_matrix(S2, B22, m);
			P3=strassen_matrix(S3, B11, m);
			P4=strassen_matrix(A22, S4, m);
			P5=strassen_matrix(S5, S6, m);
			P6=strassen_matrix(S7, S8, m);
			P7=strassen_matrix(S9, S10, m);
			
			result1=Addmatrix(Addmatrix(Addmatrix(P5, P4,m, 1),
					P2, m, -1), P6, m, 1);
			result2=Addmatrix(P1, P2, m, 1);
			result3=Addmatrix(P3, P4, m, 1);
			result4=Addmatrix(Addmatrix(Addmatrix(P5, P1,m, 1),
					P3, m, -1), P7, m, -1);
			result=TogetherMatrix(result1, result2, result3, result4, m);
		}
		return result;
	}
	//暴力矩阵相乘
	public static int[][] matrix_multiply(int[][] A,int[][] B,int n){
		int[][] result=new int[n][n];
		for(int i=0;i<n;i++){
			for(int j=0;j<n;j++){
				result[i][j]=0;
				for(int k=0;k<n;k++){
					result[i][j]+=A[i][k]*B[k][j];
				}
			}
		}
		return result;
	}
	//划分子矩阵
	public static int[][] QuarterMatrix(int[][] p,int n,int num){
		int rows=n/2;
		int cols=n/2;
		int[][] result=new int[rows][cols];
		switch(num){
			case 1:{
				for(int i=0;i<rows;i++){
					for(int j=0;j<cols;j++){
						result[i][j]=p[i][j];
					}
				}
				break;
			}
			case 2:{
				for(int i=0;i<rows;i++){
					for(int j=0;j<n-cols;j++){
						result[i][j]=p[i][j+cols];
					}
				}
				break;
			}
			case 3:{
				for(int i=0;i<n-rows;i++){
					for(int j=0;j<cols;j++){
						result[i][j]=p[i+rows][j];
					}
				}
				break;
			}
			case 4:{
				for(int i=0;i<n-rows;i++){
					for(int j=0;j<n-cols;j++){
						result[i][j]=p[i+rows][j+cols];
					}
				}
				break;
			}
			default:break;
		}
		return result;
	}
	//矩阵相加减
	public static int[][] Addmatrix(int[][] A,int[][] B,int n,int Bmark){
		int[][] result=new int[n][n];
		for(int i=0;i<n;i++){
			for(int j=0;j<n;j++){
				result[i][j]=A[i][j]+B[i][j]*Bmark;
			}
		}
		return result;
	}
	//整合矩阵
		public static int[][] TogetherMatrix(int[][] a,int[][] b,
				int[][] c,int[][] d,int n){
			int[][] result=new int[2*n][2*n];
			for(int i=0;i<2*n;i++){
				for(int j=0;j<2*n;j++){
					if(i<n){
						if(j<n){
							result[i][j]=a[i][j];
						}
						else{
							result[i][j]=b[i][j-n];
						}
					}
					else{
						if(j<n){
							result[i][j]=c[i-n][j];
						}
						else{
							result[i][j]=d[i-n][j-n];
						}
					}
				}
			}
			return result;
		}
}
```