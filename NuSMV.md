# NUSMV: 一个新的符号模型检测器

Alessandro Cimatti $^1$, Edmund Clarke $^2$, Fausto Giunchiglia $^1$, Marco Roveri $^{1,3}$

$^1$ ITC-IRST, Via Sommarive 18, 38055 Povo, Trento, Italy; E-mail: {cimatti,fausto,roveri}@irst.itc.it

$^2$ SCS, Carnegie-Mellon University, 5000 Forbes Ave., Pittsburgh, PA 15213-3891,USA; E-mail: Edmund.Clarke@cs.cmu.edu 

$^3$ DSI, University of Milano, Via Comelico 39, 20135 Milano, Italy

## 摘要

本文描述了一个新的符号模型检测器NUSMV，它是CMU和IRST联合项目的一部分。NUSMV是对CMU SMV模型检测器进行重新设计、重新实现和有限扩展的结果。本文的核心内容包括详细描述NuSMV的功能、架构和实现。

*关键词：符号模型检测 - 时序逻辑 - 自动验证 - 技术转移工具*

## 1 引言 

本文描述了卡内基梅隆大学（CMU）和科学技术研究所（IRST）之间的一个联合项目的成果，该项目的目标是开发一个新的符号模型检测器。$^1$ 这个新的模型检测器称为NUSMV，旨在成为一个结构良好、开放、灵活和有文档记录的模型检测平台。为了能在技术转移项目中使用，NUSMV被设计得非常健壮，易于修改，并接近行业要求的标准。NUSMV是对CMU SMV符号模型检测器进行重新设计和重新实现的结果。与CMU SMV相比，NUSMV在三个维度上进行了升级：

- 从系统功能的角度来看，NUSMV具有一些特性（例如，多接口、LTL规范），可以增强用户与系统交互的能力，并提供更多的启发式方法，例如，用于提高效率或部分控制状态爆炸。

- NUSMV的系统架构是高度模块化的（因此允许替换或消除某些模块）和开放的（因此允许添加新模块）。另一个特点是，在NuSMV中，用户可以控制并可能更改某些系统模块的执行顺序。

- 实现质量得到了很大提高。NUSMV是一个非常健壮且有良好文档记录的系统，其代码（相对）易于修改。

本文的组织结构如下：第2节简要介绍了符号模型检测的基本逻辑框架；第3节描述了与系统的交互；第4节解释了系统提供的功能；第5节描述了NuSMV系统架构；第6节描述了NUSMV实现特性。最后，第7节描述了一些测试结果和未来的发展方向。NUSMV可在http://afrodite.itc.it:1024/nusmv/获得。

## 2 符号模型检测

最广泛使用的验证技术是测试和仿真。然而，对于复杂的异步系统，这些技术只能覆盖可能行为的有限部分。一种补充的验证技术是时序逻辑模型检测。在这种方法中，被验证的系统被建模为有限状态转移系统，规范用命题时序逻辑表示。然后，通过穷举地探索状态转移系统的状态空间，可以自动检查规范是否得到满足。模型检测的终止由模型的有限性保证。模型检测最重要的特点之一是，当发现规范不成立时，会产生一个反例（即系统违规行为的见证）。

### 2.1 时序逻辑

有限状态系统可以描述为一个元组：

$\mathcal{M}=\langle\mathcal{S}, \mathcal{I}, \mathcal{R}, \mathcal{L}\rangle$

其中$\mathcal{S}$是有限状态集，$\mathcal{I} \subseteq \mathcal{S}$是初始状态集，$\mathcal{R} \subseteq \mathcal{S} \times \mathcal{S}$是转移关系，指定了从一个状态到另一个状态的可能转移。$\mathcal{L}$是一个函数，用给定语言中的原子命题标记状态。这样的元组称为状态转移图或Kripke结构。时序逻辑用于描述Kripke结构定义的行为。Kripke结构中的行为是从状态$s \in \mathcal{I}$开始获得的，然后通过$\mathcal{R}$重复追加可达状态。我们要求转移关系$\mathcal{R}$是全的。$^2$因此，系统的所有行为都是无限的。由于一个状态可以有多个后继状态，因此该结构可以被认为是展开成一棵无限树，表示从初始状态开始的系统的所有可能执行。图1显示了一个状态转移图及其从标记为"A"的状态展开。


图1. 状态转移图及其展开

两个有用的时序逻辑是计算树逻辑（称为CTL）和线性时序逻辑（称为LTL）。它们在处理底层计算树中的分支方式上有所不同。在CTL中，可以量化从给定状态出发的路径。在LTL中，运算符旨在描述所有可能计算路径的属性。CTL公式的语法由以下规则给出：

1. 任何原子命题都是CTL公式。

2. 如果$\alpha$和$\beta$是CTL公式，那么$\alpha \bullet \beta$和$\neg \alpha$是CTL公式，其中$\bullet$是任意布尔连接词$(\wedge, \vee, \ldots)$。

3. 如果$\alpha$和$\beta$是CTL公式，那么$\mathbf{E X} \alpha, \mathbf{E G} \alpha$，$\mathbf{E}[\alpha \mathbf{U} \beta]$是CTL公式。

图2描述了CTL公式的直观含义。$\mathbf{E X} \alpha$表示存在$(\mathbf{E})$一条从状态$s_{0} \in \mathcal{S}$开始的路径，在下一个$(\mathbf{X})$状态$\alpha$成立。$\mathbf{EG} \alpha$表示存在一条从状态$s_{0}$开始的路径，在该路径上全局$(\mathbf{G}) \alpha$成立。$\mathbf{E}[\alpha \mathbf{U} \beta]$表示存在一条从状态$s_{0}$开始的路径，在该路径上$\alpha$一直成立，直到$(\mathbf{U}) \beta$成立。其他CTL运算符（例如，$\mathbf{AF} \alpha$，表示对于所有路径最终$\alpha$成立）可以根据表1中列出的规则从这三个运算符派生。

LTL公式的语法如下：

1. 任何原子命题都是LTL公式；

2. 如果$\alpha$和$\beta$是LTL公式，那么$\alpha \bullet \beta$和$\neg \alpha$是LTL公式，其中$\bullet$是任意布尔连接词$(\wedge, \vee, \ldots)$。

3. 如果$\alpha$和$\beta$是LTL公式，那么$\mathbf{X} \alpha, \mathbf{G} \alpha,[\alpha \mathbf{U} \beta]$是LTL公式。

模型检测问题可以正式表述如下。给定一个Kripke结构$\mathcal{M}$和一个时序逻辑公式$\varphi$，找到满足$\varphi$的所有状态集合，即状态集合

$\{s \in \mathcal{S} \mid \mathcal{M}, s \models \varphi\}$

我们说，如果所有初始状态都在集合$\{s \in \mathcal{S} \mid \mathcal{M}, s \models \varphi\}$中，则系统满足规范：

$\mathcal{I} \subseteq\{s \in \mathcal{S} \mid \mathcal{M}, s \models \varphi\}$

每个CTL公式$\varphi$由其成立的状态集$\{s \in \mathcal{S} \mid \mathcal{M}, s \models \varphi\}$标识。CTL运算符可以表征为适当谓词变换器的最小或最大不动点：

- $\mathbf{EG} \alpha=\operatorname{gfp}_{Z}[\alpha \wedge \mathbf{E X} Z]$

- $\mathbf{E}[\alpha \mathbf{U} \beta]=\operatorname{lfp}_{Z}[\beta \vee(\alpha \wedge \mathbf{E X} Z)]$

这为符号模型检测算法提供了启示，该算法由公式的结构驱动。例如，$\mathbf{E}[\alpha \mathbf{U} \beta]=\operatorname{lfp}_{Z}[\beta \vee(\alpha \wedge \mathbf{E X} Z)]$的计算对应于以下迭代：

图2

图2. 时序模态的含义

表1. CTL运算符的定义

表1

$$
\begin{aligned}
& s_{0}=\perp \\
& s_{1}=\beta \vee(\alpha \wedge \mathbf{E X} \overbrace{\overbrace{\perp}^{s_{1}})}^{s_{0}})=\beta \\
& s_{2}=\beta \vee(\alpha \wedge \mathbf{E X} \overbrace{\beta}^{s_{1}}) \\
& s_{3}=\beta \vee(\alpha \wedge \mathbf{E X} \overbrace{(\beta \vee(\alpha \wedge \mathbf{E X} \beta))}^{s_{2}}) \\
& \ldots
\end{aligned} 
$$

迭代继续直到$s_{i+1}=s_{i}$。最小和最大不动点的存在性由谓词变换器的单调性和域的有限性保证。

### 2.2 Kripke结构的符号表示

第一个模型检测算法使用Kripke结构的显式表示作为标记的有向图。$^3$使用基于有序二元决策图（简称BDD）的符号表示，取得了重大改进。BDD是布尔公式的表示，一旦建立了变量的顺序，它就是规范的。图3描述了布尔公式$(a_{1} \leftrightarrow a_{2}) \wedge(b_{1} \leftrightarrow b_{2})$的BDD，使用变量顺序$a_{1}, a_{2}, b_{1}, b_{2}$。实线表示"then"弧（相应变量必须考虑为正），虚线表示"else"弧（相应变量必须考虑为负）。从根到标记为"1"的节点的路径表示所表示布尔公式的满足赋值（例如，$a_{1} \leftarrow 1, a_{2} \leftarrow 1, b_{1} \leftarrow 0, b_{2} \leftarrow 0$）。直观地说，系统的一个状态由状态变量集合的布尔值赋值符号表示。$^4$布尔公式（及其BDD）是由使公式为真的赋值表示的状态集的紧凑表示。类似地，转移关系可以表示为两组变量的布尔公式，一组与当前状态有关，另一组与下一个状态有关。
$^3$ 这几乎是正确的。然而，CesAR工具的第一个实现是一个符号模型检测器。有关更多详情，请参见。
$^4$ 对于非布尔状态变量，可以执行布尔编码。这使得可以将谓词变换器和不动点表示为BDD。基本布尔运算通过使用BDD计算布尔连接词的标准算法来处理，不动点算法可以根据基本BDD运算轻松实现。

