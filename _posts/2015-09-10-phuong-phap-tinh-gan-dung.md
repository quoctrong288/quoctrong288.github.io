---
layout: post
title: "Phương pháp tính - Chính xác hóa nghiệm"
date: 2015-09-10 00:00:00
tags: math algorithms
mathjax: true
---

* content 
{:toc}

[old post]

Tổng hợp bài tập Phương Pháp Tính, chương Giải gần đúng phương trình (chính xác hóa nghiệm)
<!--more-->

## Phương pháp chia đôi

``` c++
// Su dung pp chia doi tim nghiem gan dung
 
#include <stdio.h>
#include <conio.h>
#include <math.h>
 
#define eps 0.0001
 
float A[20];
int n;
 
void nhap_ham_so()
{
    printf("\nNhap bac cua ham so (n):");
    scanf("%d", &n);
    printf("Nhap cac he so: \n");
    for (int i = 0; i <= n; i++) {
        printf("A[%d] = ", i);
        scanf("%f", &A[i]);
    }
    return;
}
 
float tinh_ham(float c)
{
    float p = A[0];
    for (int i = 1; i <= n; i++) {
        p = p * c + A[i];
    }
    return p;
}
 
/*
float tinh_ham_sieu_viet(float c)
{
    // tu chinh sua ham: 
    return pow(e, x) + 2*x + 1;
}
*/
 
int main()
{
    float a, b, c;
    nhap_ham_so();
    do {
        printf("\nNhap khoang nghiem a, b: ");
        scanf("%f%f", &a, &b);
        if (tinh_ham(a) * tinh_ham(b) > 0) {
            printf("Kh nghiem sai, nhap lai!");
        }
        else break;
    }
    while (true);
     
    printf("\n");
    // printf(" a \t b \t\t f((a+b)/2)\n\n");
    do {
        c = (a + b) / 2;
        if (tinh_ham(c) * tinh_ham(a) < 0) b = c;
        else a = c;
        printf("%.3f \t %.3f \t %.3f\n", a, b, c);
    }
    while (fabs(a - b) > eps && tinh_ham(c) != 0);
     
    printf("\nNghiem cua pt: x = %f", c);
    return 0;
}
```

## Phương pháp lặp

``` c++
// Su dung pp lap tim nghiem gan dung   
 
#include <stdio.h>
#include <conio.h>
#include <math.h>
 
#define eps 0.0001
 
float tinh_ham(float x)
{
    // Thay ham so can tim o day:
    return pow(x + 1, 1.0 / 3);
}
 
int main()
{
    float x, y;
    printf("\nNhap x: ");
    scanf("%f", &x);
    do {
        y = x;
        x = tinh_ham(x);
        printf("\n%.3f\t%.3f", x, y);
    }
    while (fabs(x - y) > eps);
     
    printf("\n\nNghiem gan dung cua pt la: %f", x);
    return 0;
}
```

## Phương pháp tiếp tuyến

``` c++
// Su dung phuong phap tiep tuyen tim nghiem gan dung
 
#include <stdio.h>
#include <conio.h>
#include <math.h>
 
#define eps 0.0001
 
float tinh_ham(float x)
{
    // thay doi ham so o day:
    return x * x * x + x - 5;
}
 
float tinh_dao_ham(float x)
{
    // thay doi dao ham o day:
    return 3 * x * x + 1;
}
 
int main()
{
    float x, y = 0;
    printf("\nNhap x = ");
    scanf("%f", &x);
     
    do {
        y = x;
        float t = tinh_ham(y) / tinh_dao_ham(y);
        x = y - t;
        printf("\n%.3f\t%.3f", x, t);
    }
    while (fabs(x - y) > eps);
     
    printf("\n\nNghiem cua pt la: %f", y);
    return 0;
}
```

## Phương pháp dây cung

``` c++
// Su dung phuong phap day cung tinh nghiem gan dung
 
#include <stdio.h>
#include <conio.h>
#include <math.h>
 
#define eps 0.0001
 
float f(float x)
{
        // Thay doi ham so o day:
    return x * x * x + x - 5;
}
 
int main()
{
    float x, a, b;
    printf("\nNhap khoang nghiem a, b: ");
    scanf("%f%f", &a, &b);
     
    x = a - (b - a) * f(a) / (f(b) - f(a));
    if (f(x) * f(a) < 0) {
        do {
            b = x;
            x = a - ((b - a) * f(a) / (f(b) - f(a)));
            printf("\n%f\t%f\t%f\t%f", a, b, x, f(x));
        }
        while (fabs(x - b) > eps);
    }
     
    else {
        do {
            a = x;
            x = a - ((b - a) * f(a) / (f(b) - f(a)));
            printf("\n%f\t%f\t%f\t%f", a, b, x, f(x));
        }
        while (fabs(x - a) > eps);
    }
     
    printf("\nNghiem cua phuong trinh x = %f ", x);
    return 0;
}
```