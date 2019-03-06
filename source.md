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

- \\(n \in Pr\\) : $$((x+a)^n=x^n+a^n+\sum_{i=1}^{n-1}{{n \choose i}x^ia^{n-i}}$$

$${n \choose i}=\frac{n!}{i!(n-i)!}\Rightarrow n\mid {n \choose i}$$

$$(x+a)^n=x^n+a^n=x^n+a\;(mod\; n)$$

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

有\\(\forall a \le L, \; (x+a)^n = x^n + a \; (mod \; x^r-1, n )\\)

\\(\Rightarrow (x+a)^n = x^n+a \; (mod\;x^r-1,p)\\)

由\\(p\in Pr \Rightarrow (x+a)^p = x^p+a \;(mod\;x^r-1,p)\\)

\\(\Rightarrow ((x+a) ^ p)^{\frac np} = x^n+a \; (mod \; x^r-1,p)\\)

\\((x^{p} +a) ^ n = (x^p) ^ {\frac np}+a \; (mod \; x^r-1,p)\\)

\\( (x+a)^{\frac np} = x^{\frac np}+a \; (mod \; x^r-1,p)\\)

---

## 2.1 正确性证明

定义整数\\(m\\)对多项式\\(f(x)\\)是内省的，当且仅当\\(f^m(x) = f(x^m) \; (mod \; x^r-1,p)\\).

性质：

- \\(m,m'\\)分别对\\(f(x)\\)是内省的，\\(n \* n'\\)对\\( f(x) \\)也是内省的。<br/>
证明：\\(f ^ {m \* m'} (x) = ( f(x^m)) ^{m'} \; (mod \; x^r - 1, p)\\)<br/>
由\\(f^{m'}(y) = f(y^{m'}) \; (mod \; y^r - 1, p)\\)<br/>
\\(\Rightarrow (f(x ^ m)) ^ {m'} = f(x^{m\*m'}) \; mod(x^{mr}-1,p)\\)<br/>
\\(f^{m \* m'}(x) = f(x^{m \* m'}) \; (mod \; x^r-1, p)\\)

- \\(m\\)对\\(f(x),g(x)\\)分别内省，则\\(m\\)对\\(f(x)*g(x)\\)内省。<br/>
证明：\\((f(x)g(x)) ^ m = f ^ m(x)g ^ m(x) = f(x^m)  g(x^m) = fg(x^m) \; (mod \; x^r-1,p)\\)

---

## 2.1 正确性证明

由之前的证明可知：\\(\frac n p,p\\)对\\((x-a),a\in\\{0,\ldots,L\\}\\)内省。

可得\\(I=\\{(\frac np)^i \times p^j | i,j\in N\\}\\)对\\(P=\\{\prod_{a=0}^L{(x-a) ^ {k_a}} | k_a \in N \\}\\)内省。

令\\(G=\\{x \\% r| x \in I\\}\\)，设\\(|G|=t\\)，有\\(t\ge o_r(n) \gt \log^2{n}\\)

令\\(Q_r(x)\\)为\\(F_p\\)上\\(r\\)阶分圆多项式，\\(Q_r(x)\\)在域\\(F_p\\)上可分解，有\\(\frac{\phi(r)}{o_r(p)}\\)个\\(o_r(p)\\)次不可约因式，记\\(h(x)\\)为其中之一 。

记\\(F=F_p[x]/h(x)\\)，\\(D是\\) \\(\\{(x+a)|a\in \\{0,\ldots,L\\}\\}\\)在\\(F\\)中通过乘法生成的子群。

---

## 2.1 正确性证明

希望知道\\(|D|\\)的下界，下面证明\\(P\\)中\\(\lt t\\)次多项式映射到\\(D\\)中各不相同。

\\(\forall f(x)\ne g(x) \in P,\; deg(f),deg(g)\lt t\\)

假设\\(f(x)=g(x) \; (in \; F)\\)

则有\\((f(x)) ^ m = (g(x)) ^ m \; (in \; F), \quad forall \; m \in G\\) &nbsp; &nbsp; //  \\(m\lt r\\)

由\\(h(x)\mid Q_r(x) \mid (x^r-1) \Rightarrow f(x ^ m)-g(x ^ m) = 0 \; (in \; F)\\)

令\\(T(y)=f(y)-G(y)\\)，\\(T(y)\\)是以\\(F\\)为系数环的多项式。

可以发现对于任意的\\(m\in G\\)，\\(x^m\\)都是\\(T(y)\\)在\\(F\\)中的一个根。故而\\(T(y)\\)在\\(F\\)中有至少\\(t\\)个根，与次数小于\\(t\\)矛盾。

由于\\(x\\)是\\(Q_r(y)\\)在\\(F\\)中的一个根，\\((r,p)=1\\)，说明\\(F\\)中有\\(r\\)个\\(r\\)次单位根，即\\(x^m\\)在\\(F\\)中互不相同。

所以\\(|D|\ge {t+L \choose t-1}\\)

---

## 2.1 正确性证明

下证当\\(n\ne p^k\\)时，\\(|D|\le n^{\sqrt{t}}\\)

记\\(\hat{I}=\\{(\frac np) ^ ip ^ j | i,j \le \lfloor \sqrt{t} \rfloor\\}\\)，显然\\(\hat I \in I\\)。

由\\(n\ne p^k \Rightarrow |\hat I| = (\lfloor \sqrt{t} \rfloor + 1)^2 \gt t\\)

而\\(|G|=t\lt |\hat I|\\)，所以存在\\(m_1 \ne m_2 \in \hat I,\; m_1=m_2 \; (mod \; r)\\)

所以有\\(x^{m_1} = x^{m_2}\; (mod \; x^r -1)\\)

从而\\(f^{m_1}(x)=f(x ^ {m_1})=f(x ^ {m_2}) = f^{m_2}(x) \; (in \; F)\\)，对任意的\\(f(x) \in D\\)成立。

取\\(T(y)=y^{m_1}-y ^ {m_2}\\)，\\(T(y)\\)在\\(F\\)中有至少\\(|D|\\)个根，而\\(deg(T)\le (\frac np) ^{\sqrt{t}} p ^ {\sqrt{t}} \le n^{\sqrt{t}}\\)。

---

## 2.1 正确性证明

$$\begin{align\*}|G|&\ge {t+L \choose t-1} \\\\ &\ge {L+\lfloor \sqrt{t}\log n \rfloor +1 \choose \lfloor \sqrt{t}\log n \rfloor}  \qquad //\; t>\log^2n>\sqrt{t}\log n \\\\ &\ge {2\lfloor \sqrt{t}\log n \rfloor +1 \choose \lfloor \sqrt{t}\log n \rfloor} \qquad //\; t\le \phi (r) \\\\ &\gt 2^{\lfloor \sqrt{t}\log n \rfloor+1} \qquad //\; {2a+1 \choose a} = \prod{\frac{i+a}i}\gt2^a \times 2 ,\; (a>1) \\\\ &\gt n^{\sqrt{t}}\end{align\*}$$

所以当\\(n\ne p^k\\)时，算法正确。当\\(n=p ^ k\\)时，在一开始就会判断为非素数，同样正确。

---

class: center, middle

# 谢谢
