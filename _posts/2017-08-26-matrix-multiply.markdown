---
layout: post
title:  "Matrix multiply"
date:   2017-08-26 10:00:00
categories: Math
tags: math dp
excerpt: Phép nhân ma trận và ứng dụng
author: Quoc Trong Pham
mathjax: true
---

* content 
{:toc}

## Giới thiệu

Nhân ma trận là một phép tính toán nhân hai ma trận lại với nhau và tạo ra một ma trận mới là tích hai ma trận. Nó được định nghĩa tóm tắt như sau:
* Cho 2 ma trận: **$A$** kích thước **$M$** * **$N$** và **$B$** kích thước **$N$** * **$P$** (số cột của ma trận thứ nhất phải bằng số hàng của ma trận thứ hai mới thực hiện được phép nhân trên hai ma trận)
* Tích hai ma trận là ma trận **$C$** kích thước **$M$** * **$P$** được tính theo công thức: 
<br>
$$C(i, j) = \sum \mathrm A(i, k) * \mathrm B(k, j)$$
* Công thức trên được phát biểu lại như sau: Lấy các phần tử trên hàng i của matrix **$A$** nhân với các phần tử tương ứng trên cột j của matrix **$B$**, cộng tất cả lại vào phần tử hàng i cột j của matrix **$C$**.

* Để thực hiện công thức này trên máy tính, ta có thể viết thuật toán với độ phức tạp **$M$** * **$N$** * **$P$** như sau:
```c++
for (int i = 1; i <= M; i++)
	for (int j = 1; j <= P; j++) {
		C[i][j] = 0;
		for (int k = 1; k <= N; k++)
			C[i][j] += A[i][k] * B[k][j];
	}
```

## Ưng dụng

* Tùy theo từng bài toán mà phép nhân ma trận được ứng dụng khác nhau.
* Phép nhân ma trận thường được sử dụng để xử lý các bài toán yêu cầu số vòng lặp lớn, không thể duyệt được hết. Thường là $10^9$ hoặc $10^{18}$. Khi này chúng ta sẽ sử dụng phép nhân ma trận cùng với mũ nhị phân để giải quyết.

## Bài tập ví dụ

**Bài toán**


[LATGACH4](http://vn.spoj.com/problems/LATGACH4/)
<br>
Đây là một bài tập kinh điển sử dụng nhân ma trận.

**Phân tích**
<br>
Bài toán này ta có thể phân tích theo hướng quy hoạch động một cách dễ dàng như sau:
* Gọi **$F$**(N) là số cách lát 2 * N viên gạch. Khi đó, có 2 trường hợp có thể dẫn đến được **$F$**(N) đó là từ **$F$**(N-1) lát thêm 1 viên nằm đứng, hoặc từ **$F$**(N--2) lát thêm 2 viên nằm ngang.
* Như vậy công thức để tính là: `F(N) = F(N-1) + F(N-2)`
* Với **$F$**(1) = 1 và **$F$**(2) = 2 (chúng ta có thể tự tính ra điều này).

Như vậy chúng ta có thể nghĩ ngay đến việc chạy từ 1 đến N để tính. Nhưng như thế là vẫn chưa đủ, với việc chạy như vậy cùng với giới hạn N của đề cho là $10^9$ thì sẽ bị Time Limit Exceed (TLE) ngay.
<br>
Ta sẽ sử dụng cách khác. Ta xét các lớp số sau:
* **$F$**(1), **$F$**(2)
* **$F$**(2), **$F$**(3)
* ...
* **$F$**(i), **$F$**(i+1)

Từ các lớp số trước, ta có thể tính toán được các lớp số sau:
<br>
Ta có:
* $ F(i) = 0 * F(i-1) + 1 * F(i) $
* $ F(i+1) = 1 * F(i-1) + 1 * F(i) $

Từ đây ta có thể nhận thấy với mỗi lớp số ta có thể viết lại thành một ma trận 1x2, nhân với một ma trận trung gian 2x2 để tạo ra một ma trận 1x2 chính là lớp tiếp theo:

$$
        \begin{pmatrix}
        0 & 1 \\
        1 & 1 \\
        \end{pmatrix}
        x 
        \begin{pmatrix}
        F_{i-1} \\
        F_i \\
        \end{pmatrix}
        = 
        \begin{pmatrix}
        F_i \\
        F_{i+1} \\
        \end{pmatrix}
$$
<br>
Từ đây ta có được công thức tính $F_N$:
<br>
$$
		\begin{pmatrix}
        0 & 1 \\
        1 & 1 \\
        \end{pmatrix}^N
        x 
        \begin{pmatrix}
        F_0 \\
        F_1 \\
        \end{pmatrix}
        = 
        \begin{pmatrix}
        F_N \\
        F_{N+1} \\
        \end{pmatrix}
$$

Vậy là ta đã tìm được công thức, công việc còn lại là áp dụng lũy thừa nhị phân cộng với việc xử lý số lớn (giới hạn số trong máy tính là số nguyên 64 bit, sẽ có bài viết sau)

## Code

```c++
Your code goes here!
```