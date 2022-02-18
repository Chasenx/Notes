# Batch Normalization

## 1. 主要公式

- 处理对象 ---> batch 中的数据
- 处理方式 ---> batch 数据中相同维度的进行归一化，每个维度都算出对应的 $\mu$ 和 $\sigma^2$

$$
\mu_B^{(k)} = \frac{1}{m} \sum_{i=1}^{m}x_i^{(k)}
$$

$$
{\sigma^{(k)}_B}^2 = \frac{1}{m} \sum_{i=1}^{m}(x_i^{(k)} - \mu_B^{(k)})
$$

- 需要学习的参数 ---> $\gamma$ 和 $\beta$ 

$$
\hat{x}_i = \frac{x_i-\mu_B}{\sqrt{\sigma_B^2+\epsilon}}
$$

$$
y_i = \gamma \hat{x}_i + \beta
$$

## 2. 反向传播

- 需要对 $\gamma$ 和 $\beta$ 优化，所以需要求导

$$
\frac {\partial l}{\partial \gamma} = \sum_{i=1}^m\frac {\partial l}{\partial y_i} \frac {\partial y_i}{\partial \gamma} = \sum_{i=1}^m \frac {\partial l}{\partial y_i} \cdot \hat{x}_i
$$

$$
\frac {\partial l}{\partial \beta} = \sum_{i=1}^m\frac {\partial l}{\partial y_i}
$$

- 导数需要继续向前传播，所以需要对 $x_i$ 求偏导，进而涉及到对 $\mu_B$ 和 $\sigma_B^2$ 求导

$$
\frac {\partial l}{\partial \hat{x}_i} = \frac {\partial l}{\partial y_i} \cdot \gamma
$$

$$
\frac {\partial l}{\partial \sigma_B^2} = \sum_{i=1}^m\frac {\partial l}{\partial \hat{x}_i} \cdot (x_i - \mu_B) \cdot (-\frac{1}{2}) \cdot (\sigma_B^2 + \epsilon)^{-2/3}
$$

$$
\frac {\partial l}{\partial \mu_B} = (\sum_{i=1}^m \frac {\partial l}{\partial \hat{x}_i} \cdot \frac{-1}{\sqrt{\sigma_B^2+\epsilon}}) + \frac {\partial l}{\partial \sigma_B^2} \cdot \frac{\sum_{i=1}^m-2(x_i-\mu_B)}{m}
$$

$$
\frac {\partial l}{\partial x_i} = \frac {\partial l}{\partial \hat{x}_i} \cdot \frac{1}{\sqrt{\sigma_B^2+\epsilon}} + \frac {\partial l}{\partial \sigma_B^2} \cdot \frac{2(x_i-\mu_B)}{m} + \frac {\partial l}{\partial \mu_B} \cdot \frac{1}{m}
$$

## 3. 推理参数设置

- 推理的结果要求是确定的，所以需要将 $\mu_B$ 和 $\sigma_B^2$ 固定住

$$
E[x] = E_B[\mu_B] \\ 
Var[x] = \frac{m}{m-1} E_B[\sigma_B^2]
$$

- 求出所有batch中的均值的期望以及所有方差的无偏估计
