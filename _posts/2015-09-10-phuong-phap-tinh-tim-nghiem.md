---
layout: post
title: "Phương pháp tính - Tìm nghiệm"
date: 2015-09-10 00:00:00
tags: math algorithms
mathjax: true
---

* content 
{:toc}

[old post]

Tổng hợp bài tập Phương Pháp Tính, Tìm nghiệm
<!--more-->

## Phương pháp gauss

### Lưu ý
Tạo file dulieu.txt đặt chung thư mục chứa file cpp để đọc dữ liệu bằng file.
Trong file dữ liệu:
+ Hàng đầu tiên ghi giá trị n: số biến của HPT.
+ n hàng tiếp theo là các hệ số.

``` c++
#include <stdio.h>
#include <math.h>
 
#define maxN 20
 
void nhap(int *n, float A[maxN][maxN])
{
    float t;
    printf("\nNhap n = ");
    scanf("%d", &(*n));
    printf("\nNhap ma tran: \n");
    for (int i = 1; i <= *n; i++) {
        for (int j = 1; j <= *n + 1; j++) {
            scanf("%f", &A[i][j]);
        }
    }
}
 
void xuat(int n, float A[maxN][maxN])
{
    printf("\n  ===================================\n");
    for (int i = 1; i <= n; i++) {
        printf("  ");
        for (int j = 1; j <= n + 1; j++) {
            printf("%.2f\t", A[i][j]);
        }
        printf("\n");
    }
    printf("  ===================================\n");
}
 
void nhap_file(int *n, float A[maxN][maxN])
{
    FILE * f;
    f = fopen("dulieu.txt", "r");
    fscanf(f, "%d", &(*n));
    for (int i = 1; i <= *n; i++) {
        for (int j = 1; j <= *n + 1; j++) {
            fscanf(f, "%f", &A[i][j]);
        }
    }
    fclose(f);
}
 
void chuyen_matran(int n, float A[maxN][maxN], float m)
{
    for (int i = 1; i <= n - 1; i++) {
        if (A[i][i] == 0) {
            for (int j = i + 1; j <= n; j++) {
                if (A[j][i] != 0) {
                    for (int k = i; k <= n + 1; k++) {
                        float t = A[i][k];
                        A[i][k] = A[j][k];
                        A[j][k] = t;
                    }
                    break;
                }
            }
        }
         
        for (int j = i + 1; j <= n; j++) {
            m = (- A[j][i]) / A[i][i];
            for (int k = 1; k <= n + 1; k++) {
                A[j][k] = A[j][k] + A[i][k] * m;
            }
        }
    }
}
 
void tim_nghiem(int n, float A[maxN][maxN], float *X)
{
    for (int i = n; i >= 1; i--) {
        for (int k = i + 1; k <= n; k++) {
            A[i][n + 1] -= A[i][k] * X[k];
        }
        X[i] = A[i][n + 1] / A[i][i];
    }
}
 
int main()
{
    int n;
    float A[maxN][maxN], X[maxN];
    float m;
    nhap(&n, A);
     
    chuyen_matran(n, A, m);
     
    xuat(n, A);
     
    tim_nghiem(n, A, X);
     
    printf("\nNghiem cua HPT la: \n");
    for (int i = 1; i <= n; i++) printf(" %.2f\t", X[i]);
     
    return 0;
}
```

## Phương pháp Gauss Siedel

### Lưu ý

- Muốn nhập bằng file thì mở đoạn lên freopen, và tạo file input.txt chung thư mục với file .cpp rồi đưa dữ liệu vào.
- Chương trình sẽ thoát nếu lặp quá 30 lần mà chưa ra kết quả.
- Nếu có giá trị a[i][i] nào đó bằng 0 thì không giải được.
- Bài tập mẫu trong giáo trình của cô bị sai, đừng dùng nó để so sánh :)). Ở ví dụ 2 thì g phải là (1, 1, 0.8) chứ không phải (1, 1.2, 0.8).

``` c++
#include <stdio.h>
#include <math.h>
 
#define eps 0.00001
 
int n, c, tmp = 0;
float a[20][20], x[20];
 
int main()
{
    // freopen("input.txt", "r", stdin);
    printf("\nNhap n = "); 
    scanf("%d", &n);
    printf("\nNhap cac he so cua HPT: \n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n + 1; j++) {
            scanf("%f", &a[i][j]);
        }
    }
    printf("\nNhap vector nghiem ban dau: ");
    for (int i = 0; i < n; i++) scanf("%f", &x[i]);
     
    do {
        c = 0; 
        tmp++;
        for (int i = 0; i < n; i++) printf("%8.3f", x[i]);
        printf("\n");
         
        for (int i = 0; i < n; i++) {
            float s = a[i][n];
            for (int j = 0; j < n; j++) {
                if (i != j) s -= (a[i][j] * x[j]);
            }
            if (a[i][i] == 0) {
                printf("\nKhong giai duoc!");
                return 0;
            }
            float t = s / a[i][i];
            if (fabs(t - x[i]) >= eps && tmp < 30) c = 1;
            x[i] = t;
        }
    } while (c);
     
    if (tmp < 30) {
        printf("\nNghiem cua HPT la: \n");
        for (int i = 0; i < n; i++) printf("%8.3f", x[i]);
    }
    else {
        printf("\nKhong giai duoc he bang pp nay!");
    }
     
    return 0;
}
```

## Phương pháp giảm dư

### Nội dung

- Các hệ số nhập vào gồm: số phương trình n. Các hệ số của phương trình biểu diễn theo dạng ma trận, vector nghiệm ban đầu.
- Bước 1: Biến đổi hệ. Chia tất cả các phần tử ở dòng thứ i cho a[i][i];
- Bước 2: Mảng r[] chứa các số dư so sự sai khác giữa vector nghiệm ban đầu so với nghiệm thực của phương trình. Ta tìm số dư lớn nhất rồi triệt tiêu nó rồi tính lại. (xem tài liệu)
- Bước 3: Lặp lại bước 2 cho đến khi tất cả các r[i] đều bé hơn một eps nhất định.

``` c++
#include <stdio.h>
#include <math.h>
 
#define eps 0.00001
const int maxN = 15;
 
int main() 
{
    // nhap du lieu bang file:
    //freopen ("input.txt", "r", stdin);
     
    int n, t;
    float a[maxN][maxN], x[maxN], r[maxN];
     
    // Nhap:
    printf("Nhap n = ");
    scanf("%d", &n);
    printf("\nNhap he phuong trinh: \n");
    for (int i = 1; i <= n; i++)
    for (int j = 1; j <= n + 1; j++) scanf("%f", &a[i][j]);
    printf("\nNhap vector nghiem ban dau: \n");
    for (int i = 1; i <= n; i++) scanf("%f", &x[i]);
     
    // Bien doi:
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n + 1; j++) {
            if (i != j) a[i][j] /= a[i][i];
        }
        a[i][i] = 1;
    }
     
    // tinh r[i] ban dau:
    for (int i = 1; i <= n; i++) {
        r[i] = a[i][n+1];
        for (int j = 1; j <= n; j++) {
            r[i] -= a[i][j] * x[j];
        }
    }
     
    // implementation:
    do {
        for (int i = 1; i <= n; i++) printf("%8.3f", r[i]);
        printf("\n");
        t = 0; // dk dung
         
        // tim max(abs(r[i]));
        int idx = 1;
        for (int i = 2; i <= n; i++) {
            if (fabs(r[i]) > fabs(r[idx])) idx = i;
        }
        x[idx] += r[idx];
         
        // tinh lai r[i] kiem tra tiep tuc lap:
        float d = r[idx];
        for (int i = 1; i <= n; i++) {
            r[i] -= a[i][idx] * d;
            if (fabs(r[i]) >= eps) t = 1;
        }
    } while (t);
     
    printf("Nghiem cua hpt la: \n");
    for (int i = 1; i <= n; i++) printf("%8.3f", x[i]);
     
    return 0;
}
```


## Nội suy Ayken

### Giới thiệu
Bảng nội suy Ayken dùng để tính giá trị hàm tại một giá trị cho trước khi đã biết một số mốc và giá trị tương ứng tại mốc đó.

Từ các điểm cho trước và giá trị tại các điểm đó, ta xây dựng bảng nội Suy Ayken như sau:

<p align="center">
    <img src="/assets/images/ayken.jpeg">
</p>

d[i] là tích tất cả các phần tử trên dòng i.
Kết quả sẽ bằng tích các phần tử trên đường chéo chính (W(c)) nhân với tổng y[i] / d[i] (i: từ 1 tới n)


``` c++
#include <stdio.h>
#include <math.h>
 
#define eps 0.00001
const int maxN = 20;
 
int main() 
{
    //freopen("input.txt", "r", stdin);
    int n; 
    float x[maxN], y[maxN], c;
    printf("\nNhap so moc noi suy: n = ");
    scanf("%d", &n);
    printf("\nNhap moc noi suy va gia tri tuong ung");
    for (int i = 1; i <= n; i++) {
        printf("\nNhap x[%d] va y[%d]: ", i, i);
        scanf("%f%f", &x[i], &y[i]);
    }
    printf("\nNhap gia tri can tinh: ");
    scanf("%f", &c);
     
    float w = 1, s = 0, d;
    for (int i = 1; i <= n; i++) {
        w *= (c - x[i]);
        d = c - x[i];
        for (int j = 1; j <= n; j++) {
            if (i != j) d *= (x[i] - x[j]);
        }
        s += y[i] / d;
    }
    printf("\nKet qua la: %6.3f", w*s);
     
    return 0;
}
```


## Phương pháp Krame

### Giới thiệu

- Hàm dt dùng để tính ma trận vuông cấp n.
- Thay lần lượt các cột bằng cột cuối cùng để tìm nghiệm.

``` c++
#include <stdio.h>
#include <math.h>
 
#define N 100
#define eps 0.00001
 
float dt(float a[][N], int n) {
    // Ham tinh dinh thuc cua ma tran vuong cap n
    // bien doi ve ma tran tam giac
    for (int i = 1; i <= n; i++) {
        if (a[i][i] == 0) {
            for (int j = i+1; j <= n; j++) {
                if (a[j][i] != 0) {
                    for (int k = i; k <= n; k++) {
                        float t = a[i][k];
                        a[i][k] = a[j][k];
                        a[j][k] = t;
                    }
                    break;
                }
            }
        }
        for (int j = i+1; j <= n; j++) {
            float m = (-a[j][i])/a[i][i];
            for (int k = 1; k <= n; k++) {
                a[j][k] += a[i][k]*m;
            }
        }
    }
     
    // tinh dinh thuc
    float res = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            if (i == j) res *= a[i][j];
        }
    }
    return res;
}
 
int main() {
    //freopen("input.txt", "r", stdin);
    int n;
    float a[N][N], t[N][N];
    printf("\nNhap n = ");
    scanf("%d", &n);
    printf("\nNhap ma tran: \n");
    for (int i = 1; i <= n+1; i++) {
        for (int j = 1; j <= n+1; j++) {
            scanf("%f", &a[i][j]);
            t[i][j] = a[i][j];
        }
    }
     
    float D[N];
    D[0] = dt(t, n);
    if (D[0] == 0) {
        printf("Khong giai duoc he bang phuong phap nay");
        return 0;
    }
    else {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                for (int k = 1; k <= n; k++) {
                    if (k == i) t[j][k] = a[j][n+1];
                    else t[j][k] = a[j][k];
                }
            }
            D[i] = dt(t, n);
        }
        printf("He co nghiem la: \n");
        for (int i = 1; i <= n; i++) {
            printf("X[%d] = %f\n", i, D[i]/D[0]);
        }
    }
     
    return 0;
}
```

## Phương pháp Đanhilepski

### Nội dung

- Cần tìm ma trận: p1 p2 p3 – 1 0 0 – 0 1 0;
- Thực hiện n-1 lần biến đổi: Cho k chạy từ n-1 về 1. Mỗi lần cho M(-1) là ma trận đơn vị, và thay hàng thứ k thành hàng thứ k của A tính được ở lần trước. Và tính ma trận M là ma trận nghịch đảo của M(-1);
- Sau đó tính A hiện tại bằng cách: An = M(-1)*A(n-1)*M.
- Cuối cùng ta có ma trận P cần tìm. Và giá trị riêng là nghiệm của X^3 + p1*X^2 + p2*X + p3 = 0;

``` c++
#include <stdio.h>
#include <conio.h>
 
#define eps 0.00001
#define N 100
 
void nhan_matran(float a[][N], float b[][N], float c[][N], int n) {
    for (int i = 1; i <= n; i++)
    for (int j = 1; j <= n; j++) {
        c[i][j] = 0;
        for (int k = 1; k <= n; k++) {
            c[i][j] += a[i][k]*b[k][j];
        }
    }
}
 
int main() {
    freopen("input.txt", "r", stdin);
         
    int n;
    float a[N][N], M1[N][N], M[N][N], B[N][N];
    printf("\nNhap n = ");
    scanf("%d", &n);
     
    printf("\nNhap ma tran: \n");
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            scanf("%f", &a[i][j]);
        }
    }
     
    for (int k = n-1; k >= 1; k--) {
         
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (i != k) {
                    if (i == j) {
                        M[i][j] = 1;
                        M1[i][j] = 1;
                    }
                    else {
                        M[i][j] = 0;
                        M1[i][j] = 0;
                    }
                }
                else {
                    M1[i][j] = a[k+1][j];
                    if (j == k) M[i][j] = 1.0/a[k+1][k];
                    else M[i][j] = -a[k+1][j]/a[k+1][k];
                }
            }
        }
        nhan_matran(a, M, B, n);
        nhan_matran(M1,B, a, n);
    }
     
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            printf("%6.3f", a[i][j]);
        }
        printf("\n");
    }
     
    return 0;
}
```

## Phương pháp hình thang & parabol

``` c++
#include <stdio.h>
 
float f(float x) {
    // ham tinh f(x): thay doi o day
    return 1.0/(1 + x*x);
}
 
int main() {
     
    int a, b, n;
    float h, j;
    printf("\nNhap a, b, n: ");
    scanf("%d%d%d", &a, &b, &n);
     
    h = (b-a)/n;
    j = (f(a) + f(b))/2;
    for (int i = 1; i <= n-1; i++) j += f(a + i*h);
    printf("\nKQ (pp hinh thang) = %f", h*j);
     
    h = (b-a)/(2.0*n);
    j = f(a) + f(b);
    for (int i = 1; i <= 2*n - 1; i++) {
        j += (2 + 2*(i%2))*f(a + i*h);
    }
    printf("\nKQ (pp Parabol) = %f", (h*j)/(3*1.0));
     
    return 0;
}
```