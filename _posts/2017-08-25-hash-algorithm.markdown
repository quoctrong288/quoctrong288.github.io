---
layout: post
title:  "Hash algorithm"
date:   2017-08-25 00:15:00
categories: String
tags: algorithms
excerpt: Thuật toán hash xử lý chuỗi
author: Quoc Trong Pham
mathjax: true
---

* content 
{:toc}

## Sơ lược

Hash (băm) là một giải thuật sinh ra các giá trị băm tương ứng với một khối dữ liệu. Cụ thể trong giải thuật và lập trình thì đa số là sử dụng hash để lấy một đoạn mã của một chuỗi ký tự qua kiểu dữ liệu số nguyên.
Việc sử dụng Hash thì mỗi xâu ký tự sẽ có một mã Hash (thường là một số nguyên) duy nhất. Việc này sẽ giúp chúng ta rất nhiều trong việc so sánh hai xâu, hai xâu có cùng mã hash thì sẽ bằng nhau.

## Bài tập ví dụ

* Cho một đoạn văn bản gốc, gồm m ký tự.
* Cho một đoạn con, gồm n ký tự.
* Đoạn con xuất hiện bao nhiêu lần trong đoạn gốc và chỉ ra các vị trí xuất hiện đó.

Với bài toán này ta hoàn toàn có thể duyệt trâu (brute-force) từng đoạn con trong văn bản gốc rồi so sánh với đoạn con. Nhưng độ phức tạp của nó khá lớn. Hoặc có thể sử dụng thuật toán KMP  (Knuth-Morris-Pratt algorithm, thuật toán này sẽ có bài viết sau).

Bây giờ chúng ta sẽ sử dụng thuật toán hash để giải bài toán này.

## Phát biểu thuật toán

Nếu coi một ký tự là một số thì với đoạn văn bản bất kì ta có thể biểu diễn nó dưới dạng cơ số 26. Tư tưởng là đổi hai xâu từ hệ cơ số 26 ra cơ số 10, rồi đem so sánh chúng ở hệ cơ số 10.

Để tính mã hash của một xâu, ta chỉ cần đơn giản như sau:

```c++
hasP = 0; n = a.size();
for (int i = 1; i <= n; i++)
   hashP = (hashP * 26 + s[i] - 'a');
```

Tính mã hash của 1 đoạn con từ i đến j:

* Để hình dung cho đơn giản, xét ví dụ sau: Xét xâu s  và biểu diễn của nó dưới cơ số 26: (4,1,2,5,1,7,8). Chúng ta cần lấy mã Hash của đoạn con từ phần tử thứ 3 đến phần tử thứ 6, nghĩa là cần lấy mã Hash của xâu (2,5,1,7). Nhận thấy, để lấy được xâu s[3. .6], chỉ cần lấy số s[1. .6] là (4,1,2,5,1,7) trừ cho số (s[1. .2] nối thêm (0,0,0,0)) là (4,1,0,0,0,0) ta sẽ thu được (2,5,1,7). Tương tự, để lấy được mã Hash của xâu s[3. .6], chỉ cần lấy mã Hash của s[1. .6] trừ đi (mã Hash của s[1. .2] nhân với 264).
* Để cài đặt ý tưởng này, chúng ta cần khởi tạo 26x mod base (0 ≤ x ≤ m) và mã Hash của tất cả những tiền tố của s, cụ thể là mã Hash của những xâu s[1. .i] (1 ≤ i  ≤ m).
* Chúng ta sẽ tạo một mảng pow[i] (lưu lại 26i  mod base) và mảng ℎasℎt[i] (lưu lại mã Hash của T[1. . i]).

## Mã nguồn

``` c++
typedef long long ll;
const int maxN = 1e6 + 100;
const ll mod = 1e9 + 3;
const ll inf = 1e9;
 
ll POW[maxN], hashT[maxN];
string T, P;
 
ll getHash(ll i, ll j) {
return (hashT[j] - hashT[i-1]*POW[j-i+1] + mod*mod)%mod;
}
 
int main() {
    ios::sync_with_stdio(false);
     
    cin >> T >> P;
    POW[0] = 1;
    ll m = T.size(), n = P.size(), hashP = 0;
    T = " " + T;
    P = " " + P;
    for (int i = 1; i <= m; i++) 
        POW[i] = (POW[i-1]*26) % mod;
    for (int i = 1; i <= m; i++)
        hashT[i] = (hashT[i-1]*26 + T[i] - 'a') % mod;
    for (int i = 1; i <= n; i++)
        hashP = (hashP*26 + P[i] - 'a') % mod;
    for (int i = 1; i <= m-n+1; i++)
        if (hashP == getHash(i, i+n-1))
            cout << i << " ";
     
    return 0;
}
```