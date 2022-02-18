# 概率论基础

## 1. 随机变量

- 随机变量一般用无格式字体小写字母表示 $\rm x$，取值可以是 $x_{0}$ ，$x_{1}$ ，$x_{2}$ 
- 概率分布分为两种
  - 离散型 --> `PMF` (概率质量函数)
  - 连续性 --> `PDF` (概率密度函数)
- 联合概率分布
- 边缘概率 -->相当固定值其中一个变量，求另外一个随机变量的分布
- 条件概率:

$$
P({\rm y} = y \;| \;{\rm x} = x) = \frac{P({\rm y} = y , {\rm x} = x)}{P( {\rm x} = x)}
$$

- 条件概率的链式法则：

$$
P(a,b,c) = P(a|b,c)P(b|c)P(c)
$$

## 2. 独立性

- 相互独立的 --> 联合概率是可以相乘的

$$
\forall x  \in {\rm x} , \forall y  \in {\rm y} ,P({\rm x} = x,{\rm y} = y) = P({\rm x} = x)P({\rm y} = y)
$$

- 条件独立 --> 在给定条件下可以相乘

$$
\forall x  \in {\rm x} , \forall y  \in {\rm y} , \forall z  \in {\rm z} , P({\rm x} = x,{\rm y} = y \ | \  z  = {\rm z}) = P({\rm x} = x \ | \ z  = {\rm z})P({\rm y} = y \ | \ z  = {\rm z})
$$

## 3. 期望、方差、协方差

- 期望 --> $f(x)$ 关于分布 $P{\rm (x)}$ 的期望，相当于把具体函数值和对应概率值的成绩求和
- 期望是线性的
- 方差 --> $Y = f(x)-E(f(x))$ ，$Var(f(x)) = E(Y^2)$ , 函数值减去期望这个随机变量的平方的期望就是方差
- 协方差 --> 描述两个随机变量线性相关性的强度

$$
Cov(x,y) = E[(x - E[x])(y - E[y])]
$$

- 协方差为`0`只是说明两个随机变量之间没有线性关系，独立性相对更强，独立说明两个变量之间完全没有关系
- 协方差矩阵 --> 适用对象是随机向量 $\boldsymbol{x}$，对角线元素是每个分量的方差

$$
Cov({\rm x})_{i,j}=Cov(x_i,x_j)
$$

## 4. 常见概率分布

- Bernoulli 分布，0-1分布

- Multinoulli 分布，分类分布（对象是离散型随机变量）

- 指数分布:
$$
p(x;\lambda) = \lambda e^{- \lambda x},x \geq 0
$$

- 拉普拉斯分布 --> 可以看成两个指数分布合成的，$ x = \mu$ 处取得概率质量的峰值

$$
Laplace(x;\mu,\gamma) = \frac{1}{2\gamma}e^{-\frac{|x-\mu|}{\gamma}}
$$

- Dirac分布 --> 类似于信号中的单位冲击函数 $p(x) = \delta(x-\mu)$ ，经常作为经验分布

## 5. 正态分布

- 高斯分布（正态分布）--> （具有相同方差的所有可能的概率分布中，正态分布在实数上具有最大的不确定性）

$$
N(x; \mu ,\sigma ^ 2) = \frac{1}{\sqrt{2\pi } \sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}
$$

- 标准正态分布，$\mu=0, \sigma=1$

- 有时候会使用方差的倒数来控制分布 $\beta = {1}/{\sigma^2}$

- 多维正态分布 --> 参数是一个正定对称矩阵 $\Sigma$ 

$$
N(\boldsymbol{x}; \boldsymbol{\mu} ,\Sigma) = \frac{1}{\sqrt{{(2\pi)}^n \Sigma} } e^ {-\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu})^\top \Sigma ^{-1}(\boldsymbol{x}-\boldsymbol{\mu})}
$$

- 可以和一维的一样，用一个 $\Sigma$ 的逆表示一个精度矩阵 $\Beta = \Sigma ^{-1}$ 


## 6. 常用函数

- logistic sigmoid：

$$
\sigma(x) = \frac{1}{1+e^{-x}}
$$

- softplus --> Relu 的软化

$$
\zeta(x) = ln(1 + e^x)
$$

## 7. 贝叶斯公式

- 贝叶斯公式

$$
P({\rm x \ | \  y} ) = \frac{P({\rm x}) P(\rm y \ | \ x)}{P( {\rm y})}
$$

- 全概率公式

$$
P({\rm y}) = \Sigma_x  P(\rm y \ | \ x) P({\rm x})
$$

- 两个公式结合起来使用可以利用先验概率来计算后验概率

## 8. 信息论

- 自信息：

$$
I(x) = -logP(x)
$$

- log底数是 e 时，$I(x)$ 的单位是 奈特；底数是 2 时，单位是比特或香农
- 香农熵 --> 自信息的期望

$$
H(x) = E_{x\sim P}[I(x)] = -E_{x\sim P}[logP(x)]
$$

- 熵高说明不确定性更大

- KL散度：

$$
D_{KL}(P \| Q) = E_{x \sim P}[log \frac{P(x)}{Q(x)}] = E_{x \sim P}[logP(x) - logQ(x)],
\\ D_{KL}(P \| Q) \ne D_{KL}(Q \| P)
$$

- 交叉熵（Cross-Entropy）

$$
H(P,Q) = H(P) + D_{KL}(P \| Q) \\
H(P,Q) = -E_{x \sim P} log Q(x)
$$

