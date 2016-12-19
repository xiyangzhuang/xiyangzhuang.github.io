---
layout: post
title: 常见统计函数 
modified: 2016-12-17
categories: [articles, 统计]
tags: 
  - statistic
comments: true
---


### 单点分布：

$$ f(X = x)=\begin{cases}1, & x = c\\0, & x \neq c\end{cases} $$


c, 为常数， 数学期望C，方差

### 两点分布：

$$ f(X = k)=\begin{cases}p, & \text{k=0}\\p, & \text{k=1}\end{cases}  $$


数学期望p, 方差pq

### 二项分布 $$B(n,p)$$:

$$P(X=k)=C_n^kp^kq^{n-k}, k = 0,1,...,n$$

数学期望p,方差pq

### 泊松分布  $$P(\lambda)$$:

$$P(X=k)=\frac{\lambda^k}{k!}e^{-\lambda},k = 0,1,...,n, \lambda \gt 0$$

数学期望$$\lambda$$,方差$$\lambda$$


### 几何分布：

$$P(X=K) = q^{k-1}p,k = 1,2,..., 0<p<1, p+q =1$$

### 超几何分布

$$P(X=K) = \frac{C_M^kC_{N-M}^{n-k}}{C_N^n},k = 1,2,..., min(M,N), M \leq N $$

数学期望$$\frac{nM}{N}$$

方差$$\frac{nM}{N}1-\frac{M}{N}.\frac{N-n}{N-1}$$

### 帕斯卡分布

$$P(X=k)=C_{k-1}^{r-1}p^rq^{k-r}, k=r,r+1,..., 0<p<1,p+q=1$$

数学期望：$$\frac{r}{p}$$
方差：$$\frac{rq}{p^2}$$

<!-- more -->

### 常见连续型分布
均匀分布

$$ f(x)=\begin{cases}\frac{1}{b-a}, & x \in (a,b)\\0, & x \notin (a,b)\end{cases} $$

### 指数分布

$$ f(x)=\begin{cases}\lambda e^{-\lambda x}, & x \ge 0\\0, & x \lt 0\end{cases} $$

###正太分布

$$ f(x)=\frac{1}{\sqrt{2\pi} \sigma}e^\frac{(x-u)^2}{2\sigma}, -\infty < x < +\infty $$

### 对数正态分布

$$ f(x)=\begin{cases}\frac{1}{x\sigma \sqrt{2\pi}}e^\frac{(ln x-u)^2}{2\sigma}, & -\infty < x < +\infty \\0,& x \le 0 \end{cases} $$

### 威布尔分布

$$ f(x)=\begin{cases}\alpha\lambda x^{\alpha-1}e^{-\lambda x^\alpha}, & x \gt 0 \\0,& x \leq 0 \end{cases} $$

数学期望：
方差：


### 伽马分布

$$
B(m, n) = \begin{cases}\frac{\beta^\alpha}{\Gamma{(\alpha)}}x^{\alpha-1}e^{-\beta x}, 
& x \geq 0 \\0, & x \lt 0\end{cases}
$$

### 贝塔分布

$$
B(m, n) = \begin{cases}\frac{\Gamma(a+b)}{\Gamma(a)\Gamma{(b)}}x^{\alpha-1}(1-x)^{b-1}, & 0 < x < 1 \\0, \text{其它}\end{cases}
$$

### 卡方分布

$$
f(x) = \begin{cases}\frac{1}{2^{n/2}\Gamma(n/2)}
x^{n/2-1}e^{-x/2}, & x \ge 0 \\0, & x \lt 0\end{cases}
$$

### t分布

$$
f(x)=\begin{cases}\frac{\Gamma((n+1)/2)}{\sqrt{n\pi} \Gamma{(n/2)}}
, & x \ge 0 \\0, & x \lt 0
\end{cases}
$$

### F分布

$$
f(x)=\begin{cases}\frac{\Gamma((n+m)/2)}{\Gamma{(n/2)}\Gamma{(m/2)}}n^{n/2}m^{m/2}x^{n/2-1}(nx+m)^{\frac{n+m}{2}}
, & x \ge 0 \\0, & x \lt 0
\end{cases}
$$

### 柯西分布

$$ f(x)=\frac{1}{\pi}\frac{\lambda}{\lambda^2+(x-\mu)^2}, -\infty < x < +\infty $$

二维正太分布分布函数等，备注：
$$\Gamma 函数, \Gamma(\alpha)=\int_0^{+\infty} x^{\alpha-1}e^{-x}dx,\alpha \gt 0 $$
$$B(Beta) 函数, B(p,q)=\int_0^1 x^{p-1}(1-x)^{q-1}dx,p \gt 0, q \gt 0 $$