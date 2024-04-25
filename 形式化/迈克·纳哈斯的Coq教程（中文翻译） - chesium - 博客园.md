## Mike Nahas's Coq Tutorial

开始编写：2012.11.6

版本：1.2，2019.1.22

用Coq8.10+alpha测试

由Chesium翻译

开始翻译：2021.10.4

献给克尼汉（Kernighan）和里奇（Ritchie），他们为一门编程语言写了了不起的介绍

___

## 引言

Coq是个证明助手（proof assistant），可以帮助你书写形式化（formal）的证明。

一个“形式化的证明”是指一个数学证明，但用于书写它的语言类似于一门编程语言。（实际上，Coq使用的语言**就是**一门编程语言，不过这点我们待会再谈）相对普通的证明，正规的证明更难以让人类阅读，但其更易被程序所理解，这使得程序可以验证其正确性，避免人类会犯下的低级错误。

注意：验证程序本身也可能存在人类的低级错误，甚至操作系统或计算机本身也是。为降低验证程序可能出现的错误，开发者会尽可能让它短小精悍。要避免可能由验证程序导致的问题，可以同时采用多个证明检验系统来检查证明。

这篇教程会教你用Coq来书写形式化证明的基础知识。通过展现许多实在的证明过程，我会尝试将Coq强大命令功能的一部分教给你，这足以使你能够开始使用Coq了。

### 必备知识

我假设你懂得如何书写一个普通的证明，也了解一些逻辑学的知识。我会尝试使这篇教程易于阅读，所以当你对某些内容感到疑惑时，尝试接着阅读，**也许**你就能找到你想要的答案。

我也假设你已懂得至少一门编程语言，具体是哪一门并不关键。

若你觉得自己准备得不够充分，文末“延伸阅读”章节中的链接可能会对你有帮助。

### 安装Coq

最简单的方法是安装“CoqIDE”,一个Coq图形化界面的版本（确切地说是一个Coq的集成开发环境）。对于Windows和MacOS系统，安装软件（Installer）可以此网站找到：[http://coq.inria.fr/download](http://coq.inria.fr/download) 。

对于Linux系统，你的包管理器（Package Manager）大概率已经有最新版本的Coq了，对于不同版本的Linux，安装指令如下：

Debian、Ubuntu、Mint:

```shell
sudo apt install coqide
```

Fedora、CentOS可能为

```shell
su -c "yum install coq-coqide"
```

Arch Linux可能为

```shell
sudo pacman -S coqide
```

如果你喜欢文本编辑器Emacs，另外一个选择是采用`coqtop`（Coq的命令行版本）。其也可在上述网站中找到。Linux中一般称为“coq”。要在Emacs中使用它，你还需要下载Emacs模式“Proof General”。其可在此网站找到：[https://proofgeneral.github.io](https://proofgeneral.github.io/)。Linux包管理器包含其的一部分版本：（Debian、Ubuntu、Mint中为`proofgeneral`；Fedora、CentOS中可能为`emacs-common-proofgeneral`；Arch Linux中可能为`proofgeneral`）

### 加载文件

此教程的英文版使用了Coq的源代码文件格式。这类文件的扩展名通常为`.v`。有些情况下，你可能会看到`.html`或`.pdf`格式的Coq文件，那是有人用Coq生成的文档（document），这种情况下，你需要找到原始的`.v`文件。

你需要知道的是，不同的Coq文件适用于不同版本的的Coq。Coq是个研究工具，其开发者偶尔会对文件格式做一些细微的修改。为适应最新版本的Coq，你可以去作者的网站[下载](https://mdnahas.github.io/doc/nahas_tutorial.v)最新版本的教程。

## 注释

Coq会无视任何包含于`(* *)`内的字符，这些字符被称作注释（comment）。

```coq
(* 这是一条注释 *)
```

## 你的第一个证明

我们从证明以下命题开始：

> 对于所有你能证明的东西，若你有一个对其的证明，那么你能证明它。

好了，这并不是那么令人兴奋，我们不在Coq中打印传统的“Hello, World!”……

```coq
Theorem my_first_proof : (forall A : Prop, A -> A).
Proof.
intros A.
intros proof_of_A.
exact proof_of_A.
Qed.
```

### 剖分你的第一个证明

Coq证明的开始是表明你要尝试证明的东西。这由内置命令`Theorem`（定理）完成，随后是定理的名称：`my_first_proof`。如果你想重复使用这条定理，你可以随后用这个名称来指代它。接着是一个冒号`:`，以及对你想证明的定理之叙述，并以句号`.`结尾。

如你所见，所有Coq命令已句号`.`结尾。正如分号之于C++。

让我们先跳过该如何表达要证明的定理，只关注定理的证明过程本身。这些证明过程开始于`Proof`命令（没毛病）（和一个句号`.`）。然后是真正的证明过程，可以看到用了三个步骤。最后以`Qed`命令结束这个证明。

注意：不止`Theorem`，你可能会看见有一些证明开始于`Lemma`、`Remark`、`Fact`、`Corollary`或`Proposition`，这些命令都是**一样**的。在我的证明中，我只用`Theorem`。你可能也会见到有一些证明不以`Qed`结束，而是以`Admitted`或`Defined`，但这些命令的意义**不一样**，现在我们只用`Qed`。

Coq使用三种不同的”语言“， 你都可以在这个证明中见到。

-   **内嵌语言**（"vernacular" language）管理定义和顶层的用户交互。其每个命令均以一个大写字母开头，如`Theorem`、`Proof`、`Qed`。
-   **策略语言**（"tactic" language）用于书写证明。其每个命令均以一个小写字母开头，如`intros`和`exact`。
-   Coq语段（term）中的无名语言用于表达你想要证明的命题。其表达式使用了许多操作符和括号，如`(forall A : Prop, A -> A)`。（确切地说，这种语言是内嵌语言的一个子集，但将其视为另一种语言是有帮助的）

现在，我们来看看证明过程的内部，由于其可能已经滚动到了你的屏幕外侧，我再显示一次：

```coq
Theorem my_first_proof__again : (forall A : Prop, A -> A).
Proof.
intros A.
intros proof_of_A.
exact proof_of_A.
Qed.
```

### 确认你在证明中的哪个位置

CoqIDE和Proof General十分有价值，它们为你展示证明中途的情况：你已经证明了什么以及你还需证明什么。

让我们看看第一个证明中不同的情况。移动你的光标（使用鼠标或方向键）到`Proof`和`Qed`之间的任意一行上，现在我们看看在那处的证明情况：

在CoqIDE，你有三种方法

1.  在菜单栏（menu bar）打开"Navigation"（导航）菜单然后点击“go to”。
2.  在菜单栏下方的工具栏（tool bar）点击左数第五个图标（CoqIDE 8.13.2中为一个弯曲的箭头）
3.  使用键盘组合键。在作者的Mac上是`control + option + rightarrow`。

在Proof General中：

使用键盘快捷键`C-c C-Enter`（按下`control-c`然后`control-Enter`）。

在屏幕的另一侧，你应该能见到这样的一些字符

```coq
1 subgoal
A : Prop
proof_of_A : A
______________________________________(1/1)
A
```

注意中间的横线，横线上方形如`*** : ***`的所有语句都是你已知存在或作为前提存在的东西。它们被称为**假设**（hypotheses），我们把横线上方这些语句的总体称为**上下文**（the context）。在横线下方的语句是我们要尝试证明的命题，它被称作**当前子目标**（the current subgoal）。

**目标**（the goal）是我们正在尝试证明的定理。一个**子目标**（subgoal）是在证明的一个阶段或步骤中，我们正在尝试证明的命题。证明过程中我们可能有多个尚需证明的子目标，所以我们会特称“当前子目标”。举个例子，在一个使用[数学归纳法](https://baike.baidu.com/item/%E6%95%B0%E5%AD%A6%E5%BD%92%E7%BA%B3%E6%B3%95)的证明中，我们需要证明一个子目标作为归纳基础，以及另一个子目标作为归纳依据。当我们（通常）每次只考虑一个子目标的证明，而这个子目标就被称作**当前子目标**（the current subgoal）。

现在我想要解释我们使用的每一个**策略**（tactic），告诉你它们是怎么帮助证明这个定理的。这个证明可能又已滚动到屏幕之外了，所以这是它的第三次出现：

```coq
Theorem my_first_proof__again__again : (forall A : Prop, A -> A).
Proof.
intros A.
intros proof_of_A.
exact proof_of_A.
Qed.
```

### 你的第一个策略

证明开始时，我们的情况（state）是这样的：

```coq
1 subgoal
______________________________________(1/1)
forall A : Prop, A -> A
```

我们的目标（即为当前子目标）开始于`forall A : Prop,...`。用中文来描述，就是“对于所有命题A，……”。一个证明形如”对于所有整数x，……“命题的方法是：假设（assume）我们有一个任意的整数x，再证明该命题的其余部分对x成立。我们的第一个策略`intros`（引入）就是做了这件事。

这样，每次我见到`intros A`，我就想“假设“A”。

策略`intros`将`forall`从子目标的前端带出，将其指代的变量转变为上下文中一个有自己名称的**假设**（hypothesis）。回忆一下，上下文（context）保存着我们证明过的命题以及我们用于推导的假设及前提。这个假设，我们此处将其命名为`A`，与我们在子目标中移除的变量同名。我们应尽可能保持这些名称的对应关系。

**总结**：若子目标以`forall <变量名> : <类型>, ...`开头，则应用`intros <变量名>`策略。

___

当我们应用了`intros A.`后，证明情况是这样的：

```coq
1 goal
A : Prop
______________________________________(1/1)
A -> A
```

在Coq中，`A : Prop`代表你有一个名称为`A`，类型为`Prop`的东西。之后你还会遇到`0 : nat`，这指类型为`nat`（自然数）的`0`；以及`true : bool`，这指类型为`bool`（布尔值）的`true`；还有`A B C : Prop`，这说明`A`、`B`和`C`的类型均为`Prop`。  
我们执行了下一个策略后，类型`Prop`的含义会更容易解释：下一个策略还是`intros`。刚刚我们提到它适用于子目标前端的`forall`语句。其在这里也能生效，因为箭头符号`->`实际上就是`forall`的简写。这意味着`B -> C`其实是`(forall <某类型为B变量的名称> : B, C)`。看回我们的情况，`A -> A`其实是`(forall <某类型为A变量的名称> : A, A)`，我们应用的`intros proof_of_A`移除了这个隐藏的`forall`，并将那个类型为`A`的无名变量作为假设（名称为`proof_of_A`），移至了证明上下文中。

**总结**：若子目标以`<类型> -> ...`开头，则应用`intros <变量名>`策略。

___

当我们应用了第二个`intros`策略后，证明情况是这样的：

```coq
1 goal
A : Prop
proof_of_A : A
______________________________________(1/1)
A
```

现在，我们可以来聊聊`Prop`类型了。`proof_of_A`是一个证明，类型为`A`，这说明`A`是一个可以拥有证明的东西。又由`A`的类型为`Prop`，我们很容易想到`Prop`，就是**命题**（proposition）类型。

> 译注：上文这里类型为`Prop`的`A`又当作了一个类型，这个类型可以看作命题`A`的证明。也就是说，一物类型为一命题，即是指其为该命题的一个证明。

命题是个重要的概念。下面是一些命题的例子：

-   `(forall x : nat, (x < 5) -> (x < 6))`  
    对于任意自然数 x ，若我们有任意 x<5 的证明，则 x<6 。  
    换句话说，x<5 可以推导出 x<6
-   `(forall x y : nat, x + y = y + x)`  
    对于任意两个自然数 x 和 y ，有x+y\=y+x（加法交换律）
-   `(forall A : Prop, A -> A)`  
    对于任意命题 A ，若我们有任意 A 的证明，则 A 是可证明的。

上面三行都属于`Prop`类型，他们都可以拥有自己的证明。是不是觉得最后一个命题有点眼熟？这就是我们当前正在尝试证明的东西！

**注意**：千万不要把一个命题看作是对的（true）或是错的（false）。应该说，一个命题要么有证明，要么没有证明。[哥德尔](https://baike.baidu.com/item/%E5%BA%93%E5%B0%94%E7%89%B9%C2%B7%E5%93%A5%E5%BE%B7%E5%B0%94)（Kurt Friedrich Gödel）证明了有些命题不可能被证明，震惊了数学界。[塔斯基](https://baike.baidu.com/item/%E9%98%BF%E5%B0%94%E5%BC%97%E9%9B%B7%E5%BE%B7%C2%B7%E5%A1%94%E6%96%AF%E5%9F%BA)（Alfred Tarski）则更进一步，证明了一些命题甚至不能被说是正确或是错误的！为对付这些现代数学中的障碍，Coq将命题限制为已证明或未证明，而不是对或错。  
现在我讲清楚了这一点，让我们继续完成这第一个证明。刚才应用第二个`intros`策略之后，我们的子目标是`A`，这代表”我们需要一个类型为`A`的东西“，或者，`A`是个命题，也就是说”我们需要一个`A`的证明“。

前一条策略将类型就是`A`的一物（即为`A`的一个证明）移至了上下文，命名为`proof_of_A`。这样，证明上下文中的一条假设（我们已知的）的类型正好与子目标（我们要证的）相符，因此，我们说这是一个准确的匹配（exact match）。

策略`exact <假设名>`会匹配该假设和当前子目标的类型，若准确相符，则很显然证明就完成了，当前子目标就被解决了。

我们应用`exact proof_of_A`就可以完美地解决当前的子目标，这样，证明就完成了。

哈！这就是你的第一个形式化证明！

**总结**：如果子目标与某个假设相符，则使用策略`exact <假设名>`。

___

好了，我们来尝试一些更为复杂的！

## 关于含有`->`命题的证明

### 正向证明

```coq
Theorem forward_small : (forall A B : Prop, A -> (A->B) -> B).
Proof.
intros A.
intros B.
intros proof_of_A.
intros A_implies_B.
pose (proof_of_B := A_implies_B proof_of_A).
exact proof_of_B.
Qed.
```

我们看这个证明中用到的策略（在`Proof.`和`Qed.`之间），`intros`和`exact`应该很熟悉了，新来的是`pose`（产生），我们会在正向证明中使用`pose`策略。一个**正向证明**（forward proof）使用我们的前提一步步推导更复杂的已知条件，直到其与我们的目标相符。  
相对应的，一个**逆向证明**（backward proof）将我们要证明的目标拆分成更简单的子目标，直到它们简单到能显而易见地从题设中推出。  
在我们目前要证明的命题中，应用`pose`策略前的证明情况是：

```coq
1 goal
A, B : Prop
proof_of_A : A
A_implies_B : A -> B
______________________________________(1/1)
B
```

子目标是`B`，所以我们要尝试构造出一个`B`的证明。  
看到证明上下文，我们有`A_implies_B : A -> B`，回想一下，`A -> B`与`forall proof_of_A : A, B`是等价的，也就说明，对于任意`A`的证明，我们有一个`B`的证明。  
碰巧的是，我们正好也有一个`A`的证明在上下文中，叫做`proof_of_A`。表达式`A_implies_B proof_of_A`会计算出`B`的一个证明，其与那个`A`的证明是关联的。

> 译注：证明可以被计算（compute）出来，这是因为在Coq中，全称量词命题（`forall`语句）就是一个个函数，是可以被调用的。这里`A -> B`可以理解为一个函数，其接受一个`A`命题的一个证明，返回一个`B`命题的证明。具体可以参考Coq的文档。

因此，我们的`pose`策略会将`A_implies_B proof_of_A`的结果（一个`B`的证明）赋值给一个新的假设`proof_of_B`（注意：`pose`语句中讨厌的多余括号是必须的）

**总结**：如果你有一个假设为`<假设名> : <命题甲> -> <命题乙> -> ... -> <结果命题>`或是`<假设名> : forall <证明甲>:<命题甲>, (forall <证明乙>:<命题乙>, ... <结果命题> ... ))`抑或是任何箭头符号`->`与`forall`的组合，然后你的假设包含命题甲、命题乙、……的证明，则使用策略`pose`来尝试构造出结果命题的证明。

证明以`exact`策略结束，其实我们也可以直接用`exact (A_implies_B proof_of_A)`来结束证明，不过我认为用`exact proof_of_B`结尾更可读。

这就是一个正向证明，让我们来看一个逆向的。

### 逆向证明

```coq
Theorem backward_small : (forall A B : Prop, A -> (A->B)->B).
Proof.
intros A B.
intros proof_of_A A_implies_B.
refine (A_implies_B _).
exact proof_of_A.
Qed.
```

注意到我们在尝试证明与刚刚相同的命题。然而，我将要展现一个“逆向证明”，其将需证明的目标命题分解为更加简单的子目标。这里，我们一开始需要找到一个`B`的证明，但随后变为了要找一个`A`的证明，这显然更为简单。  
首先，注意刚刚正向证明用到的四个`intros`策略在这里变为了两个。`intros`策略可以输入任意数量的参数，每一个参数去掉一个子目标头部的`forall`语句，并为生成的假设命名。  
**注意**：不要使用不加参数的`intros`策略，其不会按照你的设想运行！  
我们也可以将所有`intros`语句合并为一行，不过我认为分组引入假设会使证明更为整洁。  
接下来介绍新的策略`refine`（提炼），应用`refine`策略前的证明情况是：

```coq
1 goal
A, B : Prop
proof_of_A : A
A_implies_B : A -> B
______________________________________(1/1)
B
```

当前子目标是命题`B`，所以我们要尝试构造一个`B`的证明。

我们知道`A_implies_B`可以构造出一个`B`的证明，通过已给出的`A`的证明。这个语法是`A_implies_B <某个类型为A的东西>`，策略`refine (A_implies_B _)`可以让我们不用选择特定的`A`类型参数而构造出一个`B`的证明（此处的括号是必需的）。它解决了当前的子目标，而未指定的参数（用下划线`_`表示）则成为一个新的子目标。

```coq
1 goal
A, B : Prop
proof_of_A : A
A_implies_B : A -> B
______________________________________(1/1)
A
```

我们的情况中，新的子目标让我们尝试找到一个`A`的证明。既然这是一个“子”目标，我们缩进用于解决它的策略，也就是我们的老朋友`exact`。

**总结**：若你的子目标为`<目标>`和已知条件（假设）`<假设名> : <命题甲> -> <命题乙> -> ... -> <命题N> -> <目标>`，则应用`refine (<变量名> _ _ ...)`策略，其中有 N 个下划线`_`。

逆向证明中我们会不断改变当前子目标，让其变得越来越简单，这里`A`看上去并没有比`B`简单多少，但它确实是。

现在我们增大一下难度……

### 逆向证明（复杂）

```coq
Theorem backward_large : (forall A B C : Prop, A -> (A->B) -> (B->C) -> C).
Proof.
intros A B C.
intros proof_of_A A_implies_B B_implies_C.
refine (B_implies_C _).
refine (A_implies_B _).
exact proof_of_A.
Qed.
```

我们看应用的策略序列，其开始于一对`intros`策略，然后紧跟着的是新来的`refine`块，最后由`exact`结束，这种证明模式很快会变得常见起来。

应用第一个`refine`策略前的证明情况是：

```coq
1 goal
A, B, C : Prop
proof_of_A : A
A_implies_B : A -> B
B_implies_C : B -> C
______________________________________(1/1)
C
```

我们的当前子目标是`C`，其就在`B -> C`的右端，所以我们可以使用`refine (B_implies_C _)`，这也构造出一个新的子目标`B`。  
然后，要证明`B`，我们知道`A -> B`，所以`refine (A_implies_B _)`会用`A`取代当前的子目标。  
随后，用`exact proof_of_A`结束证明。小菜一碟。

让我们来试一个更复杂的例子！

### 逆向证明（更复杂）

```coq
Theorem backward_huge : (forall A B C : Prop, A -> (A->B) -> (A->B->C) -> C).
Proof.
intros A B C.
intros proof_of_A A_implies_B A_imp_B_imp_C.
refine (A_imp_B_imp_C _ _).
exact proof_of_A.

refine (A_implies_B _).
exact proof_of_A.
Qed.
```

没错，这里有点不一样了！由`intro`开头，随后还是`refine`……但随后的策略均缩进了而且我们有两个`exact`策略！

应用第一个`refine`策略前的证明情况是：

```coq
1 goal
A, B, C : Prop
proof_of_A : A
A_implies_B : A -> B
A_imp_B_imp_C : A -> B -> C
______________________________________(1/1)
C
```

我们的当前子目标是`C`，`C`在`A -> B -> C`的最右侧，所以我们可以应用`refine (A_imp_B_imp_C _ _)`。注意到`A_imp_B_imp_C`有两个蕴含（implication）箭头符号`->`，所以`refine`需要两个下划线且构造出了两个子目标，一个要`A`的证明，一个要`B`的证明。

我说过一个“形式化的证明”是指一个数学证明，但用于书写它的语言类似于一门编程语言，这里我们能清晰地看出。`A_imp_B_imp_C`可以看作为一个需要两个参数的函数，一个类型为`A`（`A`的证明），另一个类型为`B`（`B`的证明），然后返回一个类型为`C`的值（`C`的一个证明）（上面的译注也提到了这一话题），整个函数的类型写作`A -> B -> C`，调用其的语法即为`A_imp_B_imp_C <A的一个证明> <B的一个证明>`，注意到这里不需要括号——你只需要把参数写在函数名称旁边，并用空格隔开，如`函数甲 A B`。这种风格经常用于函数式编程语言（functional programming language）如Haskell，对于更加熟悉指令式编程语言（imperative programming language）如 C 、 C++ 或 Java 的读者，你可能会觉得不使用括号和逗号区分参数十分奇怪。

第一个`refine`策略构造出两个子目标，CoqIDE 和 Proof General 会告诉你当前存在两个子目标，但其只会显示第一个子目标所对应的证明上下文。

在证明的代码中，我们用类似条件分支结构的格式来标识`refine`指令构造了多个子目标，这就像编程语言中的`if-then-else`语句或`switch/match`语句。每一个子目标的对应证明过程均被缩进了，我们又用空行来分隔它们。

证明生成的第一个子目标非常简单，我们需要一个`A`的证明，而我们有`proof_of_A : A`，`exact proof_of_A`结束了证明。随后我们放上一行空行，表示我们准备证明另一个子目标了。

证明第二个子目标的过程我们已经见过了。其也被缩进，因为其也是`refine (A_imp_B_imp_C _ _)`产生的分支之一。

看了逆向证明这个复杂定理的过程，我们再来看看在正向证明中其会是什么样。

### 正向证明（更复杂）

```coq
Theorem forward_huge : (forall A B C : Prop, A -> (A->B) -> (A->B->C) -> C).
Proof.
intros A B C.
intros proof_of_A A_implies_B A_imp_B_imp_C.
pose (proof_of_B := A_implies_B proof_of_A).
pose (proof_of_C := A_imp_B_imp_C proof_of_A proof_of_B).
exact proof_of_C.
Show Proof.
Qed.
```

这和我们刚刚证明的定理相同，不同的是它采用的是正向证明而不是逆向证明。

在这个证明中，我们能看到证明背后的编程语言。上文说过，`A_imp_B_imp_C`是一个`A -> B -> C`类型的函数。与参数`proof_of_A`和`proof_of_B`一并调用，产生了一个`C`的证明，也就是`proof_of_C`。

证明的末端有一个新的内嵌语言指令：`Show Proof`。如果你把鼠标指针放在其后端并按下`Ctrl+右箭头`（在 CoqIDE 中）或`C-C C-Enter`（在 Proof General 中），你就能看到证明过程的真正代码，其应是这样的：

```coq
(fun (A B C : Prop) (proof_of_A : A) (A_implies_B : A -> B) (A_imp_B_imp_C : A -> B -> C) =>
let proof_of_B : B := A_implies_B proof_of_A in
let proof_of_C : C := A_imp_B_imp_C proof_of_A proof_of_B in proof_of_C)
```

我们看这里的代码，`intros`策略声明了函数的参数（形参），`pose`策略声明了函数内的常量，最后，`exact`策略则返回了函数的结果。继续学习，你会看到在 Coq 中，证明过程和代码是紧密关联着的。

至此，我想说，其实 Coq 中的证明过程并不经常像上面的那些这样冗长乏味。我用这些简单的证明过程作为例子是为了向你展现 Coq 工作的机制。Coq 的策略语言包含一系列用于自动化证明和定义宏指令的策略。这篇教程中的几乎所有证明都简单到可以用一条Coq 策略直接解决。但在更加复杂的证明中，我讲到的所有策略和指令都十分关键。

许多 Coq 证明都是逆向证明，因为将目标命题转化为足够简单的子命题后，我们就可以用自动化证明策略将它们逐一解决。

目前为止，我们只与各种证明或命题打过交道，我们来尝试加入更多的数据类型吧！

## true 和 false 还是 True 和 False

内嵌指令`Inductive`（归纳式的）可让你创造出新的类型。首先是布尔（boolean）类型，其只有两种可能的值：`true`和`false`，这是我们所熟知的。但有个问题，除`true`和`false`之外，Coq 还有两个分别称为`True`和`False`的东西，其首字母大写。为了让你记住其之差别，我现在一起介绍它们。

```coq
Inductive False : Prop :=.

Inductive True : Prop :=
I : True.

Inductive bool : Set :=
| true : bool
| false : bool.
```

-   首字母大写的`False`是一个没有证明的命题。
-   首字母大写的`True`是一个命题，有一个关于其的证明叫做`I`（大写字母 I）。
-   最后，`bool`是个集合（set），有两个元素：首字母小写的`true`和首字母小写的`false`。

我知道这些名字有点令人迷惑。回忆一下，命题是可以拥有证明的东西，所以我认为首字母大写的`True`和首字母大写的`False`应该被命名为`Provable`（可证明的）和`Unprovable`（不可证明的）（或者为`AlwaysProvable`和`NeverProvable`）。首字母小写的`true`和`false`与你熟悉的布尔值是一样的。

好了，我们来对（命名糟糕的）`True`和`False`做一些证明。随后我们再转到首字母小写的`true`和`false`。

### 首字母大写的`True`和`False`

#### `True`是可证明（Provable）的

```coq
Theorem True_can_be_proven : True.
exact I.
Qed.
```

如果你去看第一行（也是仅有的一行）之前的证明情况：

```coq
1 goal
______________________________________(1/1)
True
```

没有假设，上下文是空的。我们要尝试找到一个`True`的证明。根据其定义，`True`有一个证明叫做`I`，所以，`exact I.`完成了证明。

**总结**：如果你的子目标是`True`，则应用策略`exact I.`

现在，我们转到`False`。

#### 不可证性（Unprovability）

我先前写到一个命题要么拥有一个证明，要么（尚且）没有证明。某些情况下，我们能够证明一个命题永远不能拥有证明。方法是这样的：通过说明对于任何该命题的证明，我们都可以由其推出`False`的一个证明，又由于`False`没有证明（根据其定义），所以这个命题没有证明。  
换句话说，比如我们要说明命题`A`没有证明，我们就要证明`forall proof_of_A : A, False`，或者，与其等价的`A -> False`。  
这种操作是如此常见以至于 Coq 标准库中有一个专门的运算符`~`（波浪号）来指代它。

```coq
Definition not (A:Prop) := A -> False.

Notation "~ x" := (not x) : type_scope.
```

[链接](https://coq.inria.fr/distrib/current/stdlib/Coq.Init.Logic.html#not)  
`Definition`是一个内嵌指令，用于说明两物是可替换的（interchangeable）（译注：也用于定义非递归函数）。因此，`not A`和`A -> False`是可替换的。  
`Notation`是一个内嵌指令，其用于创建运算符（此处为`~`），并将其定义为一串表达式的可选记号（notation）（此处为`not _`）。由于`not`作用于一个命题，所以`~`运算符也只能被应用于一个命题。

**注意**：Coq 标准库也是用`Notation`指令来将箭头符号`->`与`forall ...`绑定的。

来尝试证明一些命题是不可证的吧！

#### `False`是不可证的

```coq
Theorem False_cannot_be_proven : ~False.
Proof.
unfold not.
intros proof_of_False.
exact proof_of_False.
Qed.
```

这个证明中唯一的新策略是`unfold`（展开）。刚刚说过`Definition`表示两个表达式是可转换的，没错，`unfold`策略和`fold`策略会转换它们。应用`unfold`策略后，我们有：

```coq
1 goal
______________________________________(1/1)
False -> False
```

`unfold`发现`~`符号实际上是一个`->`，而我们对用`intros`来将`->`从子目标头部移除已经非常熟悉了，`intros proof_of_False.`就做了这事。

随后，与往常一样，由`intros`开始又由`exact`结束。有一个名为`proof_of_False`的假设非常奇怪，不是吗？由于我们知道`False`没有证明，所以这条假设实际上永远不可能存在。是不是直接这样说会更好一些？……

```coq
Theorem False_cannot_be_proven__again : ~False.
Proof.
intros proof_of_False.
case proof_of_False.
Qed.
```

这里证明的定理与刚刚的相同。但过程中有两点不同。

首先，其没有`unfold not.`。因为我们知道`~`就是`->`的一个简称，我们可以跳过`unfold`策略而直接使用`intros`。

**总结**：如果你的子目标为`~<...>`或`not <...>`，则采用策略`intros.`。

第二个改变是我们发现了一种新的结束证明的方法！不同于`exact`，我们使用了`case`策略。`case`十分强大：它会对其参数的每一种构造器（contructor）生成一个子目标，这里其参数为`proof_of_False`，根据定义，没有方法构造出一个`False`的证明，因此`case`没有构造出任何子目标！没有子目标，我们就完成了！

**总结**：如果你有任何假设为`<假设名> : False`，则采用策略`case <假设名>`。

#### 一些示例

用`True`和`False`，我们能看到 Coq 的箭头符号`->`表现得很像逻辑上的**蕴含**（implication） 。

```coq
Theorem thm_true_imp_true : True -> True.
Proof.
intros proof_of_True.
exact I. (** "exact proof_of_True." 也行. *)
Qed.
```

```coq
Theorem thm_false_imp_true : False -> True.
Proof.
intros proof_of_False.
exact I. (** "case proof_of_False." 也行. *)
Qed.
```

```coq
Theorem thm_false_imp_false : False -> False.
Proof.
intros proof_of_False.
case proof_of_False. (** "exact proof_of_False." 也行，但不推荐 *)
Qed.
```

`True -> False`永远不能被证明，我们可以证明`~(True -> False)`：

```coq
Theorem thm_true_imp_false : ~(True -> False).
Proof.
intros T_implies_F.
refine (T_implies_F _).
exact I.
Qed.
```

上面的证明对你而言应该都很显然了。

下面是逻辑学中的另一个主题：归谬（reduction to absurdity）。如果一个命题有一个证明，而你又证明了其不可以被证明，那你可以推出任何结论。

```coq
Theorem absurd2 : forall A C : Prop, A -> ~ A -> C.
Proof.
intros A C.
intros proof_of_A proof_that_A_cannot_be_proven.
unfold not in proof_that_A_cannot_be_proven.
pose (proof_of_False := proof_that_A_cannot_be_proven proof_of_A).
case proof_of_False.
Qed.
```

这是一个棘手的证明。由于我们的子目标`C`不在我们的假设中出现，我们不能用`exact <某个C的证明>`来结束证明。我们（目前）所知的唯一其他选项是作用在`False`上的`case`策略。

策略`unfold ... in`被用于在转换假设中`not`的定义，其发现`~A`实际上是`A -> False`，也就是一个输入一个`A`的证明，返回一个`False`的证明的函数。

了解了这个，我们就可以调用该函数，传入参数`proof_of_A`，以获得一个`False`的证明！

这是个要好好想想的证明！我们已经研究了许多有关命题的证明了，我们继续看布尔值！

### 首字母小写`true`和`false`的回归

```coq
Require Import Bool
```

`Require Import`是一个内嵌指令，其从程序库中加载定义（可类比C/C++的`#include`）。此处，加载的库叫做`Bool`，是存在一个`Bool.v`文件包含其定义、证明等等的。  
其中有两个函数定义如下：

```coq
Definition eqb (b1 b2:bool) : bool :=
match b1, b2 with
| true, true => true
| true, false => false
| false, true => false
| false, false => true
end.
```

```coq
Definition Is_true (b:bool) :=
match b with
| true => True
| false => False
end.
```

第一个函数`eqb`返回真，如果两个参数相等。（`eqb`是`equal for type bool`的缩写）  
第二个函数`Is_true`将布尔值转化为一个命题。未来，你可以使用`<名称> = true`，但目前我们仍未定义等号`=`。等号运算符`=`非常酷，但你首先得了解更多基本类型，就比如布尔值。

我们来做点关于这些函数的证明。

#### `Is_true true`是`True`

```coq
Theorem true_is_True: Is_true true.
Proof.
simpl.
exact I.
Qed.
```

`Is_true`是个函数，所以`Is_true true`是一次传入参数`true`的函数调用。由于`Is_true`的类型为`bool -> Prop`，所以我们知道函数会返回一个命题，是可以拥有证明的东西。因此，这里证明了`Is_true true`之证明的存在性。  
诚然，证明`Is_true true`看上去十分愚蠢。随后，我们会把参数`true`替换为更加有意义的布尔表达式，如`4 < 5`这样的不等式。

这个证明包含一个新的策略：`simpl`，其是`simplify`的缩写。如果你有一次函数调用，而你也有该函数的定义，那么策略`simpl`会执行该函数，传入你指定的参数。这里，函数返回了一个命题，这里为`True`，其也为一个数据类型。（没错，函数返回类型，这在 Coq 中是常见的）

`True`成为了我们新的子目标，我们知道如何证明它：使用`exact I`，因为`True`被定义为有一个名为`I`的证明的命题。

**总结**：如果当前子目标包含一次函数调用与传入的所有参数，则采用策略`simpl.`

我保证过你将会见到更复杂的`Is_true`命题，现在来了：

#### 关于复杂常量的`Is_true`证明

```coq
Theorem not_eqb_true_false: ~(Is_true (eqb true false)).
Proof.
simpl.
exact False_cannot_be_proven.
Qed.
```

策略`simpl`执行该函数然后产生了一个子目标：`~False`。你应该觉得这很熟悉，因为我们之前证明过它！  
我们可以复制一遍之前的证明，或者，我们也可以直接说：该证明已经存在了！我们先前将`~False`的证明命名为`False_cannot_be_proven`，所以策略`exact False_cannot_be_proven`可以直接完成证明。棒啊！不是吗？

现在来看一个更加复杂的`Is_true`命题，`case`策略将展现其实力！

#### `case`策略与布尔值

```coq
Theorem eqb_a_a : (forall a : bool, Is_true (eqb a a)).
Proof.
intros a.
case a.
(** 假定 a 是 true *)
simpl.
exact I.

(** 假定 a 是 false *)
simpl.
exact I.
Qed.
```

看一下应用`case a`策略后的证明状态：

```coq
2 goals
a : bool
______________________________________(1/2)
Is_true (eqb true true)
______________________________________(2/2)
Is_true (eqb false false)
```

我们有两个子目标！我之前说过`case`会为其参数的所有构造器创建一个子目标。目前，我们只对`False`类型使用过`case`。`False`类型没有构造器，所以其没有生成子目标，结束了我们的证明。  
拥有类型`bool`的假定（已知事物，这里为`a`）有两种可能的构造器：`true`和`false`，因此，`case a`创造了两个新的子目标：

-   `a`被替换为`true`
-   `b`被替换为`false`

> 译注：可以理解为分情况讨论，这里的`a`只有可能为`true`或`false`。

**注意**：`a`到`true`（或`false`）的替换只会发生在子目标中间，而不会发生在假设中。所以有时要注意在应用`case`之前控制其参数的位置。

我会通过形如`假定 <已知量> 是 <构造器>`的注释给不同的分类讨论情况贴上标签，我认为这是一种良好的证明书写风格。由于`bool`的定义中`true`被列在`false`之前，所以应用`case a`之后，`a`是`true`的情况成为了当前子目标，一旦我们证明了它，`a`是`false`的情况则会成为当前子目标。

**总结**：如果有一假定（hypothesis）（已知量）`<假定名>`的类型为一个新创建的类型，且当前子目标使用了该已知量，则你可以尝试应用策略`case <假定名>`。

我们再来一个例子：

```coq
Theorem thm_eqb_a_t: (forall a:bool, (Is_true (eqb a true)) -> (Is_true a)).
Proof.
intros a.
case a.
(** 假定 a 是 true *)
simpl.
intros proof_of_True.
exact I.

(** 假定 a 是 false *)
simpl.
intros proof_of_False.
case proof_of_False.
Qed.
```

在这个示例中，我需要在使用策略`case`前控制`a`的位置。  
一般来说，我们在应用`intros a`后应该还会应用一个`intros`，以将`->`之前的所有东西移至上下文，如果我们这么做，目标里的其中一个`a`就会被移至上下文，而`case a`就不会用`true`或`false`来替换它（`case`策略只应用于目标内）。我们就无法证明这个定理了。  
反而，我将`a`留在子目标中，将第二个`intros`策略延迟到`case a`和`simpl`之后才应用，这样，所有`a`均替换为了`true`或`false`，这个定理就能被证明了。

## “与”`and` 和 “或”`or`

Coq 中一个最令人惊奇的特点是，它的基本法则是如此简单，以至于`and`和`or`这样的基本函数都能用其定义出来。我会从`or`讲起，因为其有一些不寻常的特性。

### “或”`or`

在展示给你`or`的定义之前，我想让你看看一些例子，让其定义更加说得通。下面的例子里，想象题设里有一个条件：`x`是个自然数。

-   `or (x < 5) (x = 7)`
-   `or True False`
-   `or (x = 0) (x = 1)`

如你所见，`or`是个函数，它的每个参数均为命题——能被证明的东西，我们知道`or`产生的东西也能被证明，所以其返回值也应是个命题。命题的类型是`Prop`，所以`or`的类型定义应是：

-   `or (A B:Prop) : Prop`  
    也就是一个输入两个命题`A`、`B`，输出一个命题的二元函数。

`or`的返回值是个命题——能被证明的东西，所以我们该如何构造出一个关于`or`的证明过程？为此，我们需要看看其定义……

```coq
Inductive or (A B:Prop) : Prop :=
| or_introl : A -> A \/ B
| or_intror : B -> A \/ B
where "A \/ B" := (or A B) : type_scope.
```

这串内嵌指令做了四件事情：

-   定义了`or`：  
    一个输入两个`Prop`，输出一个`Prop`的函数
-   定义了`or_introl`：  
    一个构造器，对其输入一个`A`的证明，其会返回一个`or A B`的证明
-   定义了`or_introl`：  
    一个构造器，对其输入一个`B`的证明，其会返回一个`or A B`的证明
-   定义了`\/`：  
    一个运算符，等价于`or`。

一种理解这个定义的方式是：`or A B`创造了一个类型，而`or_introl proof_of_A`和`or_intror proof_of_B`则是该类型的实例（`or A B`的证明）。事实上，构造一个类型为`or X Y`的唯一方法就是采用构造器`or_introl`和`or_intror`。