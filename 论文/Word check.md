### 2 字级CTL

基于二进制决策图（BDDs）的符号模型检查技术已成功用于验证控制逻辑。然而，缺乏适当的表示方法来处理将布尔向量映射到整数的函数，这阻碍了该技术用于验证算术运算。我们已经尝试了前面章节中介绍的不同表示方法。不幸的是，无论是使用多终端BDD（MTBDD）还是BDD数组表示，应用于算术电路验证都存在根本性问题。对于这类应用中出现的函数，可能的值的数量是位数的指数级。因此，MTBDDs的大小也是指数级的。另一方面，对BDD数组进行算术运算非常昂贵。特别是，因为组合乘法器中间位的BDD大小是其操作数长度的指数级，所以BDD数组表示对于乘法来说是指数级的。

Bryant和Chen已经表明，BMDs为某些具有指数大小MTBDDs的函数提供了紧凑的表示。他们已经使用这种表示来验证一些算术电路的数据路径。他们能够得出电路是正确的结论，如果电路和规格的BMDs完全相同的话。然而，根据实现和控制逻辑的不同，可能存在电路正确但BMDs不相同的情况。此外，由于他们的技术无法处理不等式，因此无法检查避免Pentium错误所需的某些属性。

我们使用混合决策图（HDDs）来表示算术电路验证中出现的整数函数。特别是，对于代表数据位的状态变量，我们使用逆Reed-Muller变换，而对于代表控制信号的状态变量，我们使用恒等变换。因此，对于数据变量，这种表示表现得像BMD，而对于控制变量，它表现得像MTBDD。通过使用这种表示，我们能够处理同时具有控制逻辑和宽数据路径的电路。由于这种表示是混合决策图的一个特例，前面章节提到的所有算法都可以应用。

通过使用这种表示，我们已经扩展了符号模型检查系统SMV，使其也能处理涉及数据字之间关系的属性。在原始的SMV系统中，原子公式只能包含状态变量。在扩展的系统中，我们允许原子公式是表达式之间的等式或不等式。这些表达式表示为混合BDDs。我们使用的逻辑如下：
- 原子命题：$$Ap=\{p_1, \ldots, p_k\}$$
- 命题公式：Prop $$\::= Ap \mid Prop \wedge Prop \mid \neg Prop$$
- 字：Word $$\::= (Prop, Prop, .., Prop)$$
- 表达式：

Exp $$\::=$$ Constant $$\mid$$ Word $$\mid next($$ Word $$)\left|Exp \odot Exp_{x}\right|$$ if $$SF$$ then $$Exp$$ else $$Exp$$，其中 $$\odot$$ 可以是 +, - 或 $$\times$$。
- 原子公式：$$AF::=Ap \mid \{A \mid E\}(Exp \sim Exp)$$，其中 $$\sim$$ 可以是 $$=$$, $$<$$, 或 $$\leq$$。由于系统的非确定性行为，对于给定状态可能有多个可能的下一个状态。因此，当在表达式中使用下一个状态操作符时，需要路径量词。
- 静态公式：$$SF::=AF|SF \wedge SF| \neg SF$$
- 时态公式：$$TF::=SF|TF \wedge TF| \neg TF \mid AX TF \mid \{A \mid E\}[TF U TF]$$

模型由以下给出：
- 状态：$$S=2^{Ap}$$。
- 转换关系：$$R \subseteq S \times S$$
- 初始状态：$$S_0 \subseteq S$$
- 原子命题的赋值映射 $$V: Ap \times S \rightarrow \{0,1\}$$

逻辑的语义由以下给出：
- 命题公式：$$P: Prop \times S \rightarrow \{0,1\}$$

$$
\begin{aligned}
& P(p_i, s)=V(p_i, s) \\
& P(f_1 \wedge f_2)=P(f_1, s) \wedge P(f_2, s) \\
& P(\neg f, s)=\neg P(f, s)
\end{aligned}
$$
- 字：$$W: Word \times S \rightarrow N$$

$$
W((f_0, f_1, \ldots, f_n), s)=\sum_{i=0}^{n} P(f_i, s) 2^i
$$
- 表达式：$$E: Exp \times S \times S \rightarrow N$$。状态 $$s'$$ 用于处理表达式中出现的下一个状态操作符。

$$
\begin{aligned}
& E(e_1 \odot e_2, s, s')=E(e_1, s, s') \odot E(e_2, s, s') \\
& E(\text{if } f \text{ then } e_1 \text{ else } e_2, s, s')= \\
& \quad \text{if }(s \vDash f) \text{ then } E(e_1, s, s') \text{ else } E(e_2, s, s') \\
& E(w, s, s')=W(w, s) \\
& E(\operatorname{next}(w), s, s')=W(w, s')
\end{aligned}
$$
- 原子公式：

$$
\begin{aligned}
s \vDash & p_i \Leftrightarrow V(p_i, s)=1 \\
s \vDash & \mathbf{A}(e_1 \sim e_2) \Leftrightarrow \\
& \forall s'. R(s, s') \rightarrow (E(e_1, s, s') \sim E(e_2, s, s')) \\
s \vDash & \mathbf{E}(e_1 \sim e_2) \Leftrightarrow \\
& \exists s'. R(s, s') \wedge (E(e_1, s, s') \sim E(e_2, s, s'))
\end{aligned}
$$

公式 $$\mathbf{A}(e_1 \sim e_2)$$ 在状态 $$s$$ 为真，当 $$\left(e_1 \sim\right.$$ $$\left.e_2\right)$$ 对所有后继状态都成立。同样，$$E(e_1 \sim\right.$$ $$e_2$$ ) 在状态 $$s$$ 为真，当 $$\left(e_1 \sim e_2\right.$$ ) 对某些后继状态成立。
- SF和TF的语义与CTL中的相同。

这种逻辑可以自然地分为三层。顶层包含原子公式、静态公式和时态公式。第二层包含

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/14359925/144771b5-233f-4277-bb3e-ead44a0e54ab/240518.240640.pdf