---
layout: post
title: 决策树
---
用于分类、回归

## 1.1基础算法
![什么是决策树](https://rudadao.github.io/images/决策树.png)
 
   决策树具有互斥完备性，即样本中每个实例都被有且仅有一条路径覆盖。
   决策树是一个if-then规则集合。

* 输入:
   - 训练集:$D=\{(x_1,y_1),(x_2,y_2)...,(x_M,y_M)\}$
   - 属性集:$A=\{a_1,a_2,...,a_N\}$
* 输出：以node为结点的一棵决策树
* 过程：函数$TreeGenerate(D,A)$
>1. 生成结点node；
>2. **if** $D$中样本属于同一类别$C$ **then**
>3. 将node标记为$C$类的叶结点；**return**
>4. **endif**
>5. **if** $A=\emptyset$ **OR** $D$中的样本点在A中取值相同 **then**
>6.  将node标记为叶结点，类型为$D$中样本最多的类型；**return**
>7. **endif**
>8. 将A中选择最优划分属性$a_x$
>9. **for** $a_x$中每一个值$a^v_x$ **do**
>10.  为node生成一个分支，令$D_v$表示在$a_x$上取值为$a^v_x$的样本子集；
>11.   **if** $D_v$为空 **then**
>12.    将分支结点标记为叶结点，其类别标记为$D$中样本最多的类； **return**
>13.   **else**
>14.    以$TreeGenerate(D_v,A\\\{a_x\})$为分支结点
>15. **endif**
>14. **endfor**
   决策树的生成是一个递归过程，导致递归返回的情况有：
   1. 步骤1-3：结点中样本属于同一类，生成一个叶结点，类型为C类
   2. 步骤5-7：结点中样本因没有属性或者属性取值相同而无法划分，生成一个叶结点，类型为样本最多的类（后验分布）
   3. 步骤11-12：结点中样本为空无法划分，生成一个叶结点，类型为样本D中最多的类（先验分布）

## 1.2 特征选择（选择最优属性）
   结点尽可能属于同一类别（结点“纯度”）
### 1.2.1 信息增益
#### 1.信息熵（Information entropy）  
信息熵（Information entropy）是度量样本纯度的指标。   
$X$是一个取有限个值的离散随机变量，其概率分布为：$P(X=x_i)=p_i, i=1,2,...,N$,则离散变量$X=x_i$的自信息量为： 
$$I(x_i)=-logp_i$$  
信息熵为自信息量的期望，即:  
$$Ent(X)=E[I(x_i)]=-\sum_{i=1}^{N}p_ilogp_i$$  
信息熵的性质： 
  1. 非负性：$Ent(X)\ge0$  
  2. 最大离散熵定理： $Ent(X)\lelogN$(当$p_i=1/N时，熵最大) 
#### 2.信息增益   
样本$D$集合中第$k$类样本所占比例为$p_k(k=1,2,...,|\gamma|)$，则样本$D$的信息熵为：   
$$Ent(D)=-\sum_{k=1}^{|\gamma|}p_klogp_k$$  
假定离散属性$a$有$V$个可能取值$\{a^1,a^2,...,a^V\}$,则用$a$划分$D$产生$V$个分支，其中第$v$个分支结点，属性a的取值为$a^v，则已知属性$a$时的条件信息熵为：  
$$Ent(D|a)=-\sum_{i=1}^{M}\sum_{v=1}^{V}p(D=d_i,A=a_v)logp(D=d_i|A=a_v)$$ 
已知属性$a=a_v$的条件信息熵为：   
$$Ent(D|a=a_v)=-\sum_{i=1}^{M}\sum_{v=1}^{V}p(D=d_i|A=a_v)logp(D=d_i|A=a_v)$$  
则信息增益为：  
$$Gain(D|a)=Ent(D)-Ent(D|a)=Ent(D)-\sum_{v=1}^{V}P(a=a_v)Ent(D|a=a_v)$$  
$P(a=a_v)$为第v个分支点权重，设$D_v$第$v$个分支点的样本数量，则：  
$$ P(a=a_v)=/frac{|D_v|}{|D|}$$  
$$Ent(D|a=a_v)=Ent(D_v)$$  
$$Gain(D|a)=Ent(D)-\sum_{v=1}^{V}\frac{|D_v|}{|D|}Ent(D_v)$$ 
ID3（Irerative Dichotomiser）算法以信息增益为准则划分属性，即：
$$a_x=arg\underset{max}{a\inA} Gain(D,a)$$
 http://www.mohu.org/info/symbols/symbols.htm
