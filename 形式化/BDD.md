

## BDD
**BDD**(Binary Decision Diagram)，即二叉决策图，是一种用于表示布尔函数的数据结构。它通过DAG的形式，将布尔函数的复杂逻辑关系以图形化的方式展现出来。
## OBDD
OBDD是BDD的一种特殊形式，它是带有有序变量表的BDD，不仅使得BDD的结构变得简单统一，并且使得很多操作更加高效。
- 由OBDD的定义可知，沿一条路径不能的任意变量都不能出现多次
> $［x_1，...，*，x_n］$是⽆重复的有序变量表，B是⼀个 BDD，其所有变量都出现在该表的某处。我们说B有次序$［x_1，...，*，x_n］$，如果B的所有变量标记都出现在该表中，且 沿B中任何路径，对x，跟随x：后边的每次出现，都有i<j。
## OBDD的操作
对两个BDD操作时，它们要有相容的变量序(compatible variable order)
在《面向计算机科学的数理系统建模与推理》这本书中，OBDD的操作主要有以下几种：
1.*ruduce()* 对OBDD进行化简


![](https://raw.githubusercontent.com/ustc21xyx/picture-bed/main/20240418172757.png)





