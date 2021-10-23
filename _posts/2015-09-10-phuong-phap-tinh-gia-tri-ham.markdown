---
layout: post
title: "Phương pháp tính - tính giá trị hàm"
date: 2015-09-10 00:00:00
tags: math algorithms
mathjax: true
---

* content 
{:toc}

[old post]

Tổng hợp bài tập Phương Pháp Tính, chương Tính giá trị hàm
<!--more-->

## Tính giá trị hàm theo Hoocner

``` c++
#include <stdio.h>
 
int main()
{
    int n; 
    float c, p, a[20];
     
    printf("Nhap bac cua da thuc: "); scanf("%d", &n);
    printf("Nhap cac he so: \n");
    for (int i = 0; i <= n; i++) {
        printf("a[%d] = ", i); 
        scanf("%f", &a[i]);
    }
 
    printf("Nhap gia tri can tinh: "); 
    scanf("%f", &c);
    p = a[0];
    for (int i = 1; i <= n; i++) {
        p = p*c + a[i];
    }
    printf("Gia tri cua da thuc la : %f", p);
     
    return 0;
}
```

### Hoocner sử dụng đệ quy

``` c++
#include <stdio.h>
 
float c, a[20], p;
int n;
 
float dequy(int i)
{
    if (i == 0) return a[0];
    else return dequy(i - 1)*c + a[i];
}
 
int main()
{
    printf("Nhap bac da thuc: "); 
    scanf("%d", &n);
    printf("Nhap cac he so: "); 
    for (int i = 0; i <= n; i++) {
        printf("a[%d] = ", i);
        scanf("%f", &a[i]);
    }
     
    printf("Nhap c = "); 
    scanf("%f", &c);
     
    printf("Ket qua: %f", dequy(n));
    return 0;
}
```

## Khai triển hoocner 2 hàm

``` c++
#include <stdio.h>
int n;
float a[20], c, p;
 
void nhap_ham()
{
    printf("Nhap bac cua da thuc: "); scanf("%d", &n);
    printf("Nhap cac he so: \n");
    for (int i = 0; i <= n; i++)
    {
        printf("a[%d] = ", i); 
        scanf("%f", &a[i]);
    }
    return;
}
 
float tinh_ham(int n, int p)
{
    for (int i = 1; i <= n; i++) 
    {
        p = p*c + a[i];
    }
    return p;
}
 
int main()
{
    printf("Nhap gia tri can tinh: c = ");
    scanf("%f", &c);
    printf("Nhap ham pn: \n");
    nhap_ham();
    float A = tinh_ham(n, a[0]);
    printf("Nhap ham pm: \n");
    nhap_ham();
    float B = tinh_ham(n, a[0]); 
    printf("\nKet qua pn(c) + pm(c)= %.3f", A + B);
     
    return 0;
}
```

## Hoocner tổng quát
``` c++
#include <stdio.h>
 
int main()
{
    int n; 
    float c, a[10];
    printf("\nNhap bac da thuc: ");
    scanf("%d", &n);
     
    printf("\nNhap cac he so: \n");
    for (int i = 0; i <= n; i++) {
        printf("a[%d] = ", i);
        scanf("%f", &a[i]);
    }
     
    printf("Nhap c: (tinh p(x + c): ");
    scanf("%f", &c);
     
    for (int k = n; k >= 1; k--)
    for (int i = 1; i <= k; i++) {
        a[i] = a[i - 1]*c + a[i];
    }
     
    printf("\nCac he so moi la: \n");
    for (int i = 0; i <= n; i++) {
        printf("\na[%d] = %.3f ", i, a[i]);
    }
    return 0;
}
```

## Khai triển macloranh (maclaurin)

``` c++
#include <stdio.h>
#include <math.h>
 
#define eps 0.00001
 
float ham_cos(float x)
{
    float s = 1, d = 1;
    int i = 1;
     
    while (fabs(d) >= eps) {
        d = -d * x * x / (i * (i + 1));
        s += d;
        i += 2;
    }
     
    return s;
}
 
float ham_sin (float x)
{
    float s = x, d = x;
    int i = 2;
     
    while (fabs(d) > eps) {
        d = -d * x * x / (i * (i + 1));
        s += d;
        i += 2;
    }
     
    return s;
}
 
float ham_exp(float x)
{
    float s = 1, d = 1;
    int i = 1;
     
    while (d >= eps) {
        d = d * x / i;
        s += d;
        i++;
    }
     
    return s;
}
 
int main()
{
    float x;
    printf("\nNhap x = ");
    scanf("%f", &x);
     
    printf("\nSo sanh ket qua trong thu vien <math.h>\n");
    printf("\nCos(%.2f)= %.2f", x, ham_cos(x));
    printf("\n<math.h> kq = %.2f\n", cos(x));
    printf("\nSin(%.2f)= %.2f", x, ham_sin(x));
    printf("\n<math.h> kq = %.2f\n", sin(x));
    printf("\ne^%.2f= %.2f", x, ham_exp(x));
    printf("\n<math.h> kq = %.2f", exp(x));
     
    return 0;
}
```