class: center, middle

# Primes Is in P

&nbsp;
&nbsp;

#### 梁浩 周劭博 张子涵

---

## 内容

### 1. 约定以及基础等式

### 2. 算法正确性证明

### 3. 算法时间复杂度证明

---

## 1.1 约定

\\(mod\;h(x),n\\) : 是在环\\(Z_n[x]/h(x)\\)中的意思。

\\(O^~(f(x))\\) : \\(O(f(x)T(f(x)))\\)，其中\\(T(x)\\)是多项式。

\\(o_r(p)\\) : \\(p\\)在\\(Z_r\\)中的阶，即最小的\\(k>0,\;S.T.\;p^k=1(mod\; r)\\)。

\\(LCM(m)\\) : \\(LCM(1,2,\ldots,m )\\)。

\\(Pr\\) : 全体素数的集合。

---
name: 1.2

## 1.2 基础等式
$$\forall n\ge 2,\;(n,a)=1.$$
$$n\in Pr\Leftrightarrow \forall x, (x+a)^n=x^n+a\;(mod\;n)$$

---

template: 1.2

- \\(n \in Pr\\) : $$((x+a)^n=x^n+a^n+\sum_{i=1}^{n-1}{{n \choose i}x^ia^{n-i}}$$ $${n \choose i}=\frac{n!}{i!(n-i)!}\Rightarrow n\mid {n \choose i}$$ $$(x+a)^n=x^n+a^n=x^n+a\;(mod\; n)$$

---

template: 1.2

- \\(n \notin Pr\\) : $$\exists q \in Pr,\; q^k || n.$$ $${n \choose q}=\frac{n!}{q!(n-q)!}=\frac{n^{q\downarrow}}{q!}=C*q^{k-1},\;(C,q)=1$$  $$(a,q)=1 \Rightarrow q^k \nmid {n \choose q}a \Rightarrow n \nmid {n \choose q}a$$ $$\text{记}f(x)=(x-a)^n-a^n-a\;(mod\;n),\;f(x)\text{第}q\text{次项系数}\ne0 \Rightarrow f(x)\text{在}Z_n\text{中至多有}n-1\text{个根}。$$

---

## 1.3 公式变形

为了判断\\(n\\)是否是素数，我们需要对所有的\\(x\\)（\\(mod\;n\\)意义下）计算上面的等式，等价于直接计算左右两边的多项式是否相同。

这需要对\\(n\\)次多项式做乘法，时间复杂度甚至高于试除法。

为了知道两个多项式是否相等，我们可以在模一个任意多项式\\(g(x)\\)的意义下计算出这两个多项式，如果此时它们两个不相等，那么原本两个多项式也一定不相等。

在这里我们取\\(g(x)=x^r-1\\)，因为它的因式\\(r\\)阶分圆多项式在有限域上的分解具有很好的性质，并且足够简单可以让我们快速计算。

---

## 1.4 算法流程


1. Input: integer \\(n > 1\\).

2.  If (\\(n=a^b\\) for \\(a \in N\\) and \\(b > 1\\)), output COMPOSITE.

3.  Find the smallest r such that \\(o_r(n) > \log_2n\\).

4.  If \\(1 < (a,n) < n\\) for some \\(a \le r\\), output COMPOSITE.

5.  If \\(n <= r\\), output PRIME.

6.  For \\(a = 1\\) to \\(\lfloor \sqrt(\phi(r))\log n\rfloor\\) do <br/>
    &nbsp; &nbsp; if (\\((x + a)^n \ne x^n + a \;(mod \;x^r - 1, n)\\) ), output COMPOSITE

7. Output PRIME.

---

## 2.1 正确性证明

- \\(n\\)是素数。

我们上边的等式对素数一定成立，故而算法肯定会返回PRIME

---

## 2.1 正确性证明

- \\(n\\)不是素数。

假设此时返回PRIME，则根据算法流程，一定是在第7条语句返回，因为如果在第4条语句返回则必定是素数。此时有：

\\(o_r(n)>1 \Rightarrow \exists p \in Pr, p\mid n, p>r, o_r(p)>1\\)

记\\(L=\lfloor \sqrt(\phi(r))\log n\rfloor\\)

有\\(\forall a \le l,\;(x+a)^n=x^n+a \;(mod\;x^r-1, n)\\)

\\(\Rightarrow (x+a)^n=x^n+a\;(mod\;x^r-1,p)\\)

由\\(p\in Pr \Rightarrow (x+a)^p=x^p+a\;(mod\;x^r-1,p)\\)

\\(\Rightarrow \begin{align}((x+a)^p)^{\frac np}&=x^n+a \\ (x^p+a) \end{align}\\)
---

class: center, middle

# 谢谢
