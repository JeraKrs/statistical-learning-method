# 5.2 特征选择

#### 5.2.1 特征选择问题

特征选择在于选取对训练数据具有分类能力的特征，它将决定用哪个特征来划分特征空间。

#### 5.2.2 信息增益

**熵（entropy）**是表示随机变量不确定性的度量。

设 $$X$$ 是一个取有限个值的离散随机变量，其概率分布为 $$P(X = x_i) = p_i$$ ，则有熵（若 $$p_i = 0$$ ，则定义 $$0log0=0$$ ）

$$
H(X) = - \sum^n_{i=1}p_i\log p_i
$$

其中熵的单位为比特（bit）或纳特（nat），熵越大，随机变量的不确定性就越大，且 $$0 \leq H(p) \leq \log n$$ 。

设变量 $$(X, Y)$$ 有联合概率分布 $$P(X = x_i, Y = y_j) = p_{ij}$$ ，条件熵（conditional entropy） $$H(Y | X)$$ 表示在已知随机变量 $$X$$ 的条件，变量 $$Y$$ 的不确定性。

$$
H(Y | X) = \sum^n_{i=1} p_i H(Y | X = x_i)
$$

当条件熵中的概率由数据估计（特别是极大似然估计）得到时，所对应的熵与条件熵称为经验熵（empirical entropy）和经验条件熵（empirical conditional entropy）。

信息增益（information gain）表示得知特征 $$X$$ 的信息而使得类 $$Y$$ 的信息的不确定性减少的程度。

**定义5-2（信息增益）**

特征 $$A$$ 对训练数据 $$D$$ 的信息增益 $$g(D, A)$$ ，定义为集合 $$D$$ 的经验熵 $$H(D)$$ 与特征 $$A$$ 在给定条件下 $$D$$ 的经验条件熵 $$H(D | A)$$ 之差。

$$
g(D, A) = H(D) - H(D | A)
$$

一般情况下，熵与条件熵之差称为互信息（mutual information）。

决策树学习应用信息增益准则选择特征：给定训练数据集 $$D$$ 和特征 $$A$$ ，经验熵 $$H(D)$$ 表示对数据集 $$D$$ 进行分类的不确定性，经验条件熵 $$H(D|A)$$ 表示在特征 $$A$$ 给定的条件下对数据 $$D$$ 进行分类的不确定性。

信息增益准则的特征选择方法是：对训练数据集（或子集） $$D$$ ，计算其每个特征的信息增益，并比较它们的大小，选择信息增益最大的特征。

**算法5-1（信息增益的算法）**

输入：训练数据集 $$D$$ 和特征 $$A$$ 。

输出：特征 $$A$$ 对训练数据集 $$D$$ 的信息增益 $$g(D, A)$$ 。

1. 计算数据集的经验熵 $$H(D) = - \sum^K_{k=1} \frac{|C_k|}{|D|} \log_2\frac{|C_k|}{|D|}$$ 
2. 计算特征 $$A$$ 对数据集 $$D$$ 的经验熵条件 $$H(D | A) = \sum^{n}_{i=1}\frac{|D_i|}{|D|}H(D)$$ 
3. 计算信息增益 $$g(D, A)$$ 

#### 5.2.3 信息增益比

以信息增益作为划分训练数据集的特征，存在偏向于选择取值较多的特征的问题，而使用信息增益比（information gain ratio）可以对这一问题进行校正。

**定义5-3（信息增益比）**

特征 $$A$$ 对训练数据集 $$D$$ 的信息增益比 $$g_R(D, A)$$ 定义为其信息增益 $$g(D, A)$$ 与训练数据集 $$D$$ 的经验熵 $$H(D)$$ 之比：

$$
g_R(D, A) = \frac{g(D, A)}{H_A(D)}
$$

其中 $$H_A(D) = -\sum^n_{i=1}\frac{|D_i|}{|D|}log_2\frac{|D_i|}{|D|}$$ ， $$n$$ 是特征 $$A$$ 取值的个数。

