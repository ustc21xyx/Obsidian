#数学 

-   [由有理數定義實數](https://chenhh.gitbooks.io/mathematical_analysis/content/math_analysis/dedekind_cut.html#%E7%94%B1%E6%9C%89%E7%90%86%E6%95%B8%E5%AE%9A%E7%BE%A9%E5%AF%A6%E6%95%B8)
-   [Dedekind分劃(cut)](https://chenhh.gitbooks.io/mathematical_analysis/content/math_analysis/dedekind_cut.html#dedekind%E5%88%86%E5%8A%83cut)
-   [分劃的次序關係](https://chenhh.gitbooks.io/mathematical_analysis/content/math_analysis/dedekind_cut.html#%E5%88%86%E5%8A%83%E7%9A%84%E6%AC%A1%E5%BA%8F%E9%97%9C%E4%BF%82)
-   [分劃的加法運算](https://chenhh.gitbooks.io/mathematical_analysis/content/math_analysis/dedekind_cut.html#%E5%88%86%E5%8A%83%E7%9A%84%E5%8A%A0%E6%B3%95%E9%81%8B%E7%AE%97)
-   [分劃的乘法運算](https://chenhh.gitbooks.io/mathematical_analysis/content/math_analysis/dedekind_cut.html#%E5%88%86%E5%8A%83%E7%9A%84%E4%B9%98%E6%B3%95%E9%81%8B%E7%AE%97)

## [](https://chenhh.gitbooks.io/mathematical_analysis/content/math_analysis/dedekind_cut.html#%E7%94%B1%E6%9C%89%E7%90%86%E6%95%B8%E5%AE%9A%E7%BE%A9%E5%AF%A6%E6%95%B8)由有理數定義實數

-   實數形成的集合$\mathbb{R}$ 配備了次序關係、加法運算與乘法運算後，稱之為實數系(real number system)。
    
-   一般定義實數是由自然數$\mathbb{N}$ 為基礎，搭配Peano 公理來定義實數系。
    
-   以下將由有理數(rational number)為一個有序體且正整數(natural number)具有良序性的基礎，來證明實數系(real number system)為一個具有完備性的有序體，因此需以下假設，且分為三部份。
    
    -   定義何謂實數。
    -   定義實數的次序關係、加法與乘法運算，並證明有序體的性質對實數也成立。
    -   實數的完備性。
-   定義：假設有理數為有序體(ordered field) ($\mathbb{Q}, +, \times$ )，且正整數$\mathbb{N}$ 有良序性。令$\mathbb{Q}^{+} \subseteq \mathbb{Q}$ 為正有理數且$\forall p,\ q,\ r  \in \mathbb{Q}^{+}$ 滿足以下性質。-   (加法交換性)$p + q = q + p$ .
    -   (加法結合性)$(p+q)+r = p+(q+r)$ .
    -   (加法單位元素)$\exists 0 \in \mathbb{N} \ni p + 0 = p$ .
    -   (加法單位反元素)$\forall p \in \mathbb{Q}^{+} \ \exists -p \in \mathbb{Q}^{+} \ni p + (-p) = 0$ .
    -   (乘法交換性)$p \times q  = q \times p$ .
    -   (乘法結合性)$(p \times q) \times r = p \times (q \times r)$ .
    -   (乘法單位元素)$\exists 1 \in \mathbb{N} \ni p \times 1 = p$ .
    -   (乘法單位反元素)$\forall p \neq 0, \exists p^{-1} \ni p \times p^{-1} = 1$ .
    -   (分配律) $p \times (q + r) = p \times q + p \times r$ .
    -   (三一律) $\forall p$ ，三者中必定恰有一成立：$p \in \mathbb{Q}^{+}, \ -p \in \mathbb{Q}^{+},\ p = 0$ .
    -   (加法封閉性)$p \in \mathbb{Q}^{+} \text{ and } q \in \mathbb{Q}^{+} \Rightarrow p + q \in \mathbb{Q}^{+}$ .
    -   (乘法封閉性)$p \in \mathbb{Q}^{+} \text{ and } q \in \mathbb{Q}^{+} \Rightarrow p \times q \in \mathbb{Q}^{+}$ .
    -   (正整數的良序性) 若$\phi \neq S \subseteq \mathbb{N}$ ，則$S$ 必定有最小元素。
    

-   為了證明實數系為完備的有序體，這裡採用的是Richard Dedekind所提出的方法。
    
-   在早期證明定理都是以幾何圖形來說明，因此若將「直線有連續性」解釋為在直線上，任意兩(有理)點都有其它(有理)點存在，其實不夠嚴謹。因為我們很容易可畫出無理數$r, \ r^2 = 2$的圖形。
    
-   $Theorem: r^2 = 2$ 不為有理數。
    
    -   Proof:
    -   Let $p \in \mathbb{Q} \text{ and } p^2 = 2$.
    -   $p = m/n,\ \forall m,n \in \mathbb{N} \text{ and gcd}(m,n) = 1$ .
    -   $\therefore m^2 = 2n^2 \Rightarrow m^2$ 為偶數，所以$m$ 為偶數。
    -   令 $m = 2 m_1$ ，可得 $n^2 = 2 m_1^2$，所以$n$ 也為偶數。
    -   因此可得$\text{gcd}(m,n) = 2$ 矛盾。(QED)。
-   定義：**分劃(cut)**-   令$\phi \neq S \subset \mathbb{Q}$ ，若
    -   $\forall p \in S, S=\{q \in \mathbb{Q} | q < p \}$ ；即比$p$ 小的有理數均為$S$的元素
    -   $\forall p \in S \ \exists q \in \mathbb{Q} \ni q > p \text{ and } q \in S$ ，即集合$S$ 存在有理數上界為集合的元素。
    -   若集合$S$ 滿足以上性質時，稱$S$ 為$\mathbb{Q}$的一個分劃。
    
    -   此定義即為比某個數小的全體有理數所形成的集合。
    -   E.g. 給定$r \in \mathbb{Q}$ ，則$r^{*}=\{ q \in \mathbb{Q} | q < r \}$ 為有理數$r$ 所決定的有理數分劃(rational cut)。
    -   E.g. 給定$d \in \mathbb{Q}^{+}$ ，則$S = \{p \in \mathbb{Q} | p \leq 0 \} \cup \{ p \in \mathbb{Q} | p > 0 \text{ and } p^2 < d \}$ 為一分劃。
        -   Proof:
        -   $\because 0 \in S, \therefore S \neq  \phi$。且$(1+d)^2 > d, \therefore 1+d \notin S \therefore S \neq \mathbb{Q}$.
        -   若$p \in S$ 且$q \in \mathbb{Q},\ q < p$ 。
            -   當$q \leq 0$時，可知$q \in S$。
            -   當$q > 0$ 時，$\because 0 < q < p \therefore q^2 < p^2 < d \therefore q \in S$.
        -   若$p \in S$ ，要證明$S$ 存在上界，只須考慮$p > 0$ 的情形即可。
            -   令 $q = (p^3 + 3dp)/(3p^2 + d) \in \mathbb{Q}$且
            -   $q-p = \frac{p^3 + 3dp}{3p^2 + d}-p = \frac{2p(d-p^2)}{3p^2 + d} > 0$.
            -   $d-q^2 = d - \frac{(p^3+3dp)^2}{(3p^2+d)^2} = \frac{(d-p^2)^3}{(3p^2+d)^2} > 0$ .
            -   $\therefore q > p \text{ and } q \in S$ (QED).
-   對於任何分劃$S$ ，均有以下兩個性質：
    
    -   $\forall p \in S, q \in \mathbb{Q},\text{ and } q \notin S \Rightarrow q > p$.(分劃必存在有理數上界)
        
    -   $\forall p,q \in \mathbb{Q},\ p \notin S \text{ and } p < q \Rightarrow q \notin S$ .(分劃必包含小於某個有理數的全部有理數)
        

## [](https://chenhh.gitbooks.io/mathematical_analysis/content/math_analysis/dedekind_cut.html#%E5%88%86%E5%8A%83%E7%9A%84%E6%AC%A1%E5%BA%8F%E9%97%9C%E4%BF%82)分劃的次序關係

-   定義：**相等分劃**-   $S_1, S_2 \subset \mathbb{Q}$為有理數中的分劃，若兩集合相等時，記為$S_1 = S_2$ 。
    
-   定義：**分劃的大小**-   $S_1, S2 \subset \mathbb{Q}$ 為有理數中的分劃。
    -   若存在有理數$p$ 滿足 $p \in S_1$ 且 $p \notin S_2$ ，則記為$S_1 > S_2$ 或 $S_2 < S_1$ .
    -   $S_1 \geq S_2$ 表示$S_1 > S_2$ 或 $S_1 = S_2$ .
    
-   Theorem:分劃的次序滿足三一律-   $S_1, S_2 \subset \mathbb{Q}$為有理數的分劃，則下列三者恰有一成立。
    -   $S_1 > S_2,\ S_1 < S_2, S_1 = S_2$ .
    
-   Theorem:分劃的次序滿足遞移律-   $S_1, S_2, S_3 \subset \mathbb{Q}$ 為有理數的分劃。
    -   $S_1 > S_2 \text{ and } S_2 > S_3 \Rightarrow S_1 > S_3$ .
    
    -   Proof:
    -   $\because S_1 > S_2, \therefore \exists p \in \mathbb{Q} \ni p \in S_1 \text{ and } p \notin S_2$.
    -   $\because S_2 > S_3, \therefore \exists q \in \mathbb{Q} \ni q \in S_2 \text{ and } q \notin S_3$.
    -   $\because p \notin S_2 \text{ and } q \in S_2 \therefore p > q$.
    -   $\because q \notin S_3 \text{ and } p > q \therefore p \notin S_3$.
    -   $\because p \in S_1 \text{ and } p \notin S_3 \Rightarrow S_1 > S_3$ (QED).

## [](https://chenhh.gitbooks.io/mathematical_analysis/content/math_analysis/dedekind_cut.html#%E5%88%86%E5%8A%83%E7%9A%84%E5%8A%A0%E6%B3%95%E9%81%8B%E7%AE%97)分劃的加法運算

-   Theorem: 兩分劃可以相加。-   $S_1, S_2 \subset \mathbb{Q}$ 為有理數的分劃。
    -   則集合$S_3 = \{ p+q | p \in S_1,\ q \in S_2 \}$ 也是有理數的分劃。
    
    -   Proof:
    -   \[1\] 證明$\phi \neq S_3  \subset \mathbb{Q}$。
    -   $\because S_1 \neq \phi \text{ and } S_2 \neq \phi \therefore S_3 \neq \phi$ .
    -   $\because S_1 \neq \mathbb{Q} \text{ and } S_2 \neq \mathbb{Q} \therefore \exists s,t \in \mathbb{Q} \ni s \notin S_1 \text{ and } t \notin S_2$.
    -   因此可得$\forall p \in S_1,\ p < s$, $\forall q \in S_2,\  q < t$ .
    -   $\therefore \forall v \in S_3, v < s+t \text{ and} s+t \notin S_3,\ S_3 \neq \mathbb{Q}$ \[1\].
    -   \[2\] 證明比$p+q$小的所有有理數均為$S_3$ 的元素。
    -   Let $v \in S_3, \ s \in \mathbb{Q}, \ s < v$ .
    -   依$S_3$ 的定義，必存在$p \in S_1,\ q \in S_2 \ni v = p + q$ .
    -   令$t = s - q$ ，則 $t \in \mathbb{Q}$ ，且因$s < v$ 可得$t < p$ .
    -   $\because S_1$ 是分劃且$p \in S_1$ ，所以$t \in S_1$ 。
    -   $\because s = t + q$ 且 $q \in S_2$ ，所以$s \in S_2$ \[2\]。
    -   \[3\] 證明$S_3$ 存在有理數上界。
    -   令 $v \in S_3$ ，則可找到$p \in S_1 \text{ and } q \in S_2 \ni v = p + q$.
    -   $\because S_1$是分劃且$p \in S_1$，所以可找到$s \in S_1 \ni s > p$。
    -   $\therefore s + q \in S_3 \text{ and } s + q > v$ \[3\].
    -   由\[1,2,3\]可得$S_3$為有理數中的分劃(QED)。
-   定義：分劃的和(sum)-   $S_1, S_2 \subset \mathbb{Q}$ 為有理數的分劃，則$S_1, S_2$ 之和為$S_1 + S_2$
    -   $S_1 + S_2 = \{p+q | p \in S_1,\ q \in S_2  \}$ .
    
-   Theorem:分劃的加法滿足交換律-   $S_1, S_2 \subset \mathbb{Q}$ 為有理數的分劃，則$S_1 + S_2 = S_2 + S_1$
    
-   Theorem:分劃的加法滿足結合性-   $S_1, S_2,S_3 \subset \mathbb{Q}$ 為有理數的分劃，則$(S_1 + S_2) + S_3 = S_1 + (S_2 + S_3)$ .
    
-   Theorem:分劃的加法有單位元素-   $S \subset \mathbb{Q}$ 為有理數的分劃，則$S + 0 = S$.
    
-   Theorem:每個分劃都有加法反元素-   $S \subset \mathbb{Q}$ 為有理數的分劃，則必存在一個分劃$T$ 滿足$S+T = 0$ .
    
    -   分劃$S$ 的加法反元素將以$-S$ 表示。
    -   加法反元素具唯一性。
-   Corollary:逼近定理-   $S \subset \mathbb{Q}$ 為有理數的分劃，則對每個正有理數$p$ ，必可找到$s \in S$ 以及$q \in \mathbb{Q} - S$ 且 $q$ 不是$\mathbb{Q} -S$ 的最小元素，使得$q-s=p$
    
    -   E.g. 令$S=1/2$ 的分劃，若$p=3,\ s=0$ ，則$q=3$ .
-   Theorem:分劃的加法可保持次序-   $S_1, S_2,S_3 \subset \mathbb{Q}$ 為有理數的分劃。
    -   若$S_1 > S_2 \Rightarrow S_1 + S_3 > S_2 + S_3$ 。
    

## [](https://chenhh.gitbooks.io/mathematical_analysis/content/math_analysis/dedekind_cut.html#%E5%88%86%E5%8A%83%E7%9A%84%E4%B9%98%E6%B3%95%E9%81%8B%E7%AE%97)分劃的乘法運算

-   Theorem:兩正分劃可以相乘-   $S_1, S_2 \subset \mathbb{Q}$ 為有理數的分劃，且$S_1 > 0,\ S_2 > 0$ 。
    -   則$\{ r \in \mathbb{Q} | r \leq 0 \} \cup \{ pq | p \in S_1 \cap \mathbb{Q}^{+},\ q \in S_ \cap \mathbb{Q}^{+}\}$ 為有理數中的分劃。
    -   稱為正分劃$S_1, S_2$ 的積(product)，記為$S_1 S_2$ 。
    

-   定義：分劃的積-   $S_1, S_2 \subset \mathbb{Q}$ 為有理數的分劃，則積$S_1 S_2$ 定義如下：
    -   $S_1 = 0,\ S_2 = 0,\ S_1 S_2 = 0$ .
    -   $S_1 > 0,\ S_2 < 0,\ S_1 S_2 = -(S_1(-S_2))$ .
    -   $S_1 < 0,\ S_2 > 0,\ S_1 S_2 = -((-S_1) S_2)$ .
    -   $S_1 < 0,\ S_2 < 0,\ S_1 S_2 = (-S_1)(-S_2)$ .
    -   $S_1 > 0,\ S_2 > 0,\ S_1 S_2$ 同前一個定理。
    
-   Theorem:分劃的乘法滿足交換律-   $S_1, S_2 \subset \mathbb{Q}$ 為有理數的分劃。
    -   則$S_1 S_2 = S_2 S_1$ 。
    
-   Theorem:分劃的乘法滿足結合律-   $S_1, S_2, S_3 \subset \mathbb{Q}$ 為有理數的分劃。
    -   則$(S_1 S_2) S_3 = S_1 (S_2 S_3)$ 。
    
-   Theorem:分劃的乘法有單位元素-   $S \subset \mathbb{Q}$ 為有理數的分劃，則$S 1 = S$。
    
-   Theorem:每個不為0的分劃都有乘法反元素-   $0 \neq S \subset \mathbb{Q}$ 為有理數的分劃。
    -   則必有一分劃$T$ 滿足$ST=1$
    
    -   $T$ 為分劃$S$ 的乘法反元素，記為$S^{-1}$ 或$1/S$ 。
-   Theorem:逼近定理-   $S \subset \mathbb{Q}$ 為有理數的一個正分劃。
    -   則對於每個大於1的有理數$p$ ，必可找到$s \in S,\ q \in \mathbb{Q} - S$且$q$ 不是$\mathbb{Q}-S$ 的最小元素使得$p=q/s$
    
-   Theorem:分劃的乘法對加法可分配-   $S_1, S_2, S_3 \subset \mathbb{Q}$ 為有理數的分配。
    -   則$S_1(S_2 + S_3) = S_1 S_2 + S_1 S_3$ .