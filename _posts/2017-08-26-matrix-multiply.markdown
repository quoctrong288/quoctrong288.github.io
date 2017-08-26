---
layout: post
title:  "Matrix multiply"
date:   2017-08-26 20:00:00
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
* Cho 2 ma trận: $A kích thước $M * $N và $B kích thước $N * $P (số cột của ma trận thứ nhất phải bằng số hàng của ma trận thứ hai mới thực hiện được phép nhân trên hai ma trận)
* Tích hai ma trận là ma trận C kích thước $M * $P được tính theo công thức: 
$$
$C(*i*, *j*) = Sum($A(*i*, *k*) * $B(*k*, *j*)))
$$ 

* "# Header 1"
* "## Header 2"
* "### Header 3"
* "#### Header 4"
* "##### Header 5"
* "###### Header 6"

* *in nghiêng*
* **in đậm**
* `đóng khung`


## Danh sách
* List 1
	* Sublist 1
	* Sublist 2
* List 2

Number list:
1. apples
2. oranges
3. somethings

## Hình ảnh, link

* Sử dụng thẻ img trong html để resize

<p><img src="https://i.pinimg.com/736x/bc/f0/4e/bcf04eafebdf707b8d900f02e6d8bd70--photo-tag-touch-me.jpg" width="200"></p>
* Không thể chỉnh kích thước ảnh trên internet -_-
![demo_image](https://i.pinimg.com/736x/bc/f0/4e/bcf04eafebdf707b8d900f02e6d8bd70--photo-tag-touch-me.jpg)

* [demo_link_1](https://facebook.com/quoctrong.qb)

* [demo_link_nay][demo_link]

[demo_link]: https://facebook.com/quoctrong.qb



## Nhúng code

``` c++
#include <iostream>

int main() {
	std::cout << "HELLO WORLD!";
	return 0;
}
```

### Tạo trích dẫn
* Một developer nổi tiếng ở Trung Quốc đã từng nói:
> 前端发展很快，现代浏览器原生已经足够好用。我们并不需要为了操作
* Một cách khác để tạo danh sách: 
`[+]`

## Tạo bảng

|Cột 1|Cột 2|Cột 3|
|------|-------|-------|
|Hàng 1|1x2|1x3|
|Hàng 2|2x2|2x3|
|Hàng 3|3x2|3x3|

## Tạo chú thích

* Chú thích[^1]

[^1]: Là cái gì đó, nó luôn luôn nằm ở dưới cùng.

Chi tiết hơn ở link sau: [LINK](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

## Tạo biểu thức toán học
* Sử dụng Mathjax

$$
f(x) = ax + b
$$

* Inline Mathjax $a \neq b$
