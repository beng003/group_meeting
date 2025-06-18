 定义 2（ORE）： 顺序揭示加密（ORE）方案是一个由三个算法构成的元组 $\Pi = (\mathcal{K}, \mathcal{E}, \mathcal{C})$，其语法定义如下：

---

- **密钥生成算法 $\mathcal{K}$：**
	- 性质：随机化算法
	- 输入：安全参数 $\lambda$ 的二进制表示 $1^\lambda$ 和明文空间上限 $M$
	- 输出：始终生成两个密钥——加密密钥 sk 和 比较密钥 ck。

- **加密算法 $\mathcal{E}$：**
	- 性质：随机化算法
	- 输入：秘密密钥 sk 和明文 $m$（其中 $m \in [M] = \{0,1,\dots,M-1\}$）
	- 输出：单个密文 $c$。

- **比较算法 $\mathcal{C}$：**
	- 性质：确定性算法
	- 输入：比较密钥 ck 和两个密文 $c_1, c_2$
	- 输出：一个比特位（表示比较结果）。

---
特殊情形： 若比较算法 $\mathcal{C}$ 是简单整数比较（即：将密文 $c_1, c_2$ 解释为整数的二进制表示，并直接比较大小），则该方案称为保序加密（OPE）方案。

---

### 关键术语解析

| 符号                                 | 含义                                                                  |
| ---------------------------------- | ------------------------------------------------------------------- |
| $D_{\text{aff}}^{\delta, \ell}$    | 仿射变换后的分布：对原始分布 $D$ 进行线性变换，将 $[0,1]$ 映射到 $[\ell, \ell + \delta]$ 区间。 |
| $\left\lfloor \cdot \right\rfloor$ | 取整操作：将连续分布离散化为整数集（需满足明文空间约束）。                                       |
| $\delta, \ell$                     | 操作参数：$\delta$ 控制缩放倍数（拉伸程度），$\ell$ 控制移位量（平移位置）。                      |
| $\gamma$                           | 安全阈值：限制敌手选择的缩放强度，避免通过“无穷小缩放”破解（$\delta < \gamma$ 时游戏直接中止）。          |


---

### 平滑CLWW泄漏剖面
非自适应泄漏剖面 $\mathcal{L}_f$ 的定义： 该泄漏剖面以消息向量 $\boldsymbol{m}=(m_1,\ldots,m_q)$ 为输入，输出以下信息：

$$\mathcal{L}_f(m_1,\ldots,m_q) := \Big( \forall 1\leq i,j,k\leq q,\ \ \mathbb{1}(m_i < m_j),\ \mathbb{1}\big(\mathsf{msdb}(m_i,m_j) = \mathsf{msdb}(m_i,m_k)\big) \Big)$$

$\mathcal{L}_f$ 的核心特性：

1. 泄漏量严格小于CLWW：

- 除明文顺序关系 $\mathbb{1}(m_i < m_j)$ 外，仅额外泄漏 $\mathsf{msdb}$ 的位置是否相同（而非具体位置值）。

2. 左移不变性（平滑性）：

- 若将所有明文左移一位（即 $m_i \leftarrow 2m_i$），泄漏结果不变：

$$\mathcal{L}_f(\boldsymbol{m}) = \mathcal{L}_f(2\boldsymbol{m})$$

- 此性质表明 $\mathcal{L}_f$ 是平滑CLWW泄漏剖面。

---
CLWW的$\mathcal{L}_{clww}$ 泄漏剖面
$$\mathcal{L}_\mathrm{clww}(m_1,\ldots,m_q):=(\forall1\leq i,j\leq n,\mathbf{1}(m_i<m_j),\mathsf{msdb}(m_i,m_j))$$



---
### 游戏 $\text{Game }(\gamma, D)\text{-para - hid}_{\Pi,q}(\mathcal{A},\lambda)$ 的交互步骤如下：

1. **初始化：**
- 生成密钥 $(\text{sk}, \text{ck}) \leftarrow \mathcal{K}(1^\lambda, M)$，其中 $M$ 为明文空间上限。
- 敌手 $\mathcal{A}$ 获取公参数 $(\gamma, D)$ 和比较密钥 $\text{ck}$。

2. **挑战阶段：**

- $\mathcal{A}$ 提交两对参数 $(\delta_0, \ell_0)$ 和 $(\delta_1, \ell_1)$ 给挑战者 $\mathcal{C}$。
- $\mathcal{C}$ 抛硬币选择 $b \in \{0,1\}$，并检查参数有效性：需满足 $\delta_0 \geq \gamma$ 且 $\delta_1 \geq \gamma$；否则随机返回结果（中止游戏）。

3. **加密与响应：**

- 从 取整的仿射分布 $\left\lfloor D_{\text{aff}}^{\delta_b, \ell_b} \right\rfloor$ 中采样 $q$ 个明文 $\boldsymbol{m}$，要求 $\max \boldsymbol{m} \leq M$。
- 计算密文 $\boldsymbol{c} = \mathcal{E}(\text{sk}, \boldsymbol{m})$ 并发送给 $\mathcal{A}$。

---
4. **敌手猜测：**

- $\mathcal{A}$ 基于 $\boldsymbol{c}$ 输出比特 $b'$。
- 胜败判定：若 $b' = b$ 则敌手获胜。

####  安全优势定义

$\mathbf{Adv}_{\Pi, q, \gamma, D}^{\text{para - hid}}(\mathcal{A}, \lambda) = \left| \Pr \left[ \text{敌手获胜} \right] - \frac{1}{2} \right|$

- 安全性要求：若对任意高效敌手 $\mathcal{A}$ 和多项式 $q$，上述优势均为可忽略函数（neglibile function），则称 ORE 方案 $\Pi$ 是 $(\gamma, D)$-参数隐藏的。

---

 **$(\eta,\mu)$-平滑分布**：设 $D$ 是一个支撑集主要位于 $[0,1]$ 上的概率分布（即满足 $\Pr [x \notin [0,1]: x \leftarrow D] \leq \text{negl}(\lambda)$），$p_D(x)$ 表示其概率密度函数，$p'_D(x)$ 为其导数。称 $D$ 是 $(\eta,\mu)$-平滑的，若满足：
  (1) 密度有界：$\forall x \in [0,1]$, $p_D(x) \leq \eta$；
  (2) 导数有界（允许有限例外）：在 $[0,1]$ 上除至多 $\mu$ 个点外，处处满足 $|p'_D(x)| \leq \eta$。

---

#### 连续分布的变换操作

设 $D$ 为连续随机变量 $X$ 的分布，$p_D(x)$ 为其概率密度函数。定义三类变换：

| 变换类型 | 概率密度函数                                                                              | 操作意义                       |
| ---- | ----------------------------------------------------------------------------------- | -------------------------- |
| 缩放分布 | $p_{D_{\text{scale}}^{\delta}}(x) = \frac{1}{\delta} p_D(\frac{x}{\delta})$         | 将分布形状横向缩放 $\delta$ 倍       |
| 平移分布 | $p_{D_{\text{shift}}^{\ell}}(x) = p_D(x - \ell)$                                    | 将分布向右平移 $\ell$ 个单位         |
| 仿射分布 | $p_{D_{\text{aff}}^{\delta,\ell}}(x) = \frac{1}{\delta} p_D(\frac{x-\ell}{\delta})$ | 同时进行缩放 $\delta$ 和平移 $\ell$ |

> 注：仿射分布是缩放与平移的复合操作，即 $D_{\text{aff}}^{\delta,\ell} = (D_{\text{scale}}^{\delta})_{\text{shift}}^{\ell}$。

---
#### 离散化取整分布（解决明文为整数的约束）

对定义在实数区间 $[\alpha, \beta]$ 上的分布 $D$，其取整分布 $R_D^{\alpha,\beta}$ 定义如下：

- 采样方式：从 $D$ 中采样实数 $x$ → 取整为 $\lfloor x \rfloor$。
- 概率质量函数（分段定义）：

$$p_{R_D^{\alpha,\beta}}(k) = \begin{cases} \dfrac{ \int_{\alpha}^{\lceil\alpha\rceil + 0.5} p_D(x) dx }{ \int_{\alpha}^{\beta} p_D(x) dx } & k = \lceil\alpha\rceil \\ \dfrac{ \int_{k-0.5}^{k+0.5} p_D(x) dx }{ \int_{\alpha}^{\beta} p_D(x) dx } & k \in (\lceil\alpha+1\rceil, \lfloor\beta-1\rfloor) \\ \dfrac{ \int_{\lfloor\beta\rfloor - 0.5}^{\beta} p_D(x) dx }{ \int_{\alpha}^{\beta} p_D(x) dx } & k = \lfloor\beta\rfloor \\ 0 & \text{其他} \end{cases}$$


---
**离散化原理说明：**

1. **边界处理：**
- 左端点 $k=\lceil\alpha\rceil$：积分范围为 $[\alpha, \alpha+0.5]$（覆盖首个整数的左侧半区间）。
- 右端点 $k=\lfloor\beta\rfloor$：积分范围为 $[\beta-0.5, \beta]$（覆盖末位整数的右侧半区间）。
2. **中间点：**
- 每个整数 $k$ 对应实数区间 $[k-0.5, k+0.5)$，概率为 $p_D(x)$ 在该区间的积分。
3. **归一化：**
- 分母 $\int_{\alpha}^{\beta} p_D(x) dx$ 保证总概率为 1（需注意 $\alpha,\beta$ 可能使支撑集不完整

##### 符号简化

对三类变换的取整分布简记为：

 $\left\lfloor D_{\text{scale}}^{\delta} \right\rfloor$（缩放取整）； $\left\lfloor D_{\text{shift}}^{\ell} \right\rfloor$（平移取整）； $\left\lfloor D_{\text{aff}}^{\delta,\ell} \right\rfloor$（仿射取整）

---

#### **参数隐藏可排序加密（Parameter-Hiding ORE）的形式化描述**

预先设定参数：  

$q=\text{poly}(\lambda), \quad M=2^{\text{poly}(\lambda)}, \quad \gamma=2^{\omega(\log\lambda)}, \quad \eta,\mu\leq O(1)$

>假设$\gamma$和$M$经向上取整后为精确的$2$的幂。定义：  

$\tau=\gamma, \quad \xi=\gamma^{2}, \quad U=4\xi M, \quad T=\gamma^{2}\times U, \quad K=2\times T$

 设$\Pi=(\mathcal{K},\mathcal{E},\mathcal{C})$为消息空间$[K]$上具有**平滑化CLWW泄露**$\mathcal{L}$的ORE方案。  
定义消息空间$[M]$上的新ORE方案$\Pi_{\text{aff}}=(\mathcal{K}_{\text{aff}},\mathcal{E}_{\text{aff}},\mathcal{C}_{\text{aff}})$如下：

---
#### **密钥生成、加密与比较算法**

- $\mathcal{K}_{\text{aff}}(1^{\lambda},M,\Pi)$：  
  - 输入安全参数$\lambda$、消息空间$[M]$和$\Pi$。选取超多项式全局参数$\gamma=2^{\omega(\log\lambda)}$，计算上述参数。运行$(ck,sk)\leftarrow\mathcal{K}(1^{\lambda},K)$，从$\log U(\xi,2\xi-1)$抽取$\alpha$，从$[T]'$的离散均匀分布抽取$\beta$。输出$sk_{\text{aff}}=(sk,\alpha,\beta)$和$ck_{\text{aff}}=ck$。  
- $\mathcal{E}_{\text{aff}}(sk_{\text{aff}},m)$：  
  - 输入密钥$sk_{\text{aff}}$和消息$m\in[M]$，输出密文：
  - $CT_{\text{aff}}=\mathcal{E}(\alpha m + \beta)$（基于$\Pi$的消息空间$[K]$设计，输入必在有效域内）。  

- $\mathcal{C}_{\text{aff}}(ck_{\text{aff}},CT_{\text{aff}}^{0},CT_{\text{aff}}^{1})$：  
  - 输入比较密钥$ck_{\text{aff}}$和两个密文，输出$\mathcal{C}(ck_{\text{aff}},CT_{\text{aff}}^{0},CT_{\text{aff}}^{1})$。

---
**附：简化方案定义**  
仅实现**缩放隐藏**或**平移隐藏**的组合方案形式化定义为：  
$\Pi_{\text{scale}}=(\mathcal{K}_{\text{scale}},\mathcal{E}_{\text{scale}},\mathcal{C}_{\text{scale}})$和$\Pi_{\text{shift}}=(\mathcal{K}_{\text{shift}},\mathcal{E}_{\text{shift}},\mathcal{C}_{\text{shift}})$。

#### **简化方案的具体实现**

$\Pi_{\text{scale}}$**（缩放隐藏）**：  
  - **密钥生成**：类似$\Pi_{\text{aff}}$，但仅抽取$\alpha \stackrel{\$}{\leftarrow} \log U(\xi,2\xi-1)$，输出$sk_{\text{scale}}=(sk,\alpha)$。  
  - **加密**：$CT_{\text{scale}} = \mathcal{E}(\alpha m)$。  
  - **比较**：直接调用$\Pi$的比较算法。
  
$\Pi_{\text{shift}}$**（平移隐藏）**：  
  - **密钥生成**：抽取离散均匀分布$\beta \leftarrow [T]'$，输出$sk_{\text{shift}}=(sk,\beta)$。  
  - **加密**：$CT_{\text{shift}} = \mathcal{E}(m + \beta)$。  
  - **比较**：直接调用$\Pi$的比较算法。

$\Pi_{\text{aff}}$,$\Pi_{\text{scale}}$,$\Pi_{\text{shift}}$的**正确性**由$\Pi$的正确性保证，核心价值在于其**隐私性保障**。

---

### 一、核心内容

 定理7（主定理）： 若 ORE 方案 $\Pi$ 满足 $\mathcal{L}$-模拟安全性（其中 $\mathcal{L}$ 为平滑 CLWW 泄漏剖面），则对于任意 超多项式参数 $\gamma = 2^{\omega(\log\lambda)}$，方案 $\Pi_{\text{aff}}$ 是 $(\gamma,\eta,\mu)$-参数隐藏的。

- $\gamma$ 约束：缩放参数需足够大（$\gamma$ 超多项式增长，如 $\gamma > 2^{\lambda}$）
- $\mathcal{L}$ 要求：泄漏剖面需满足 平滑CLWW性质（抵抗比特级分析攻击）


| 步骤  | 参数过渡                                   | 依赖性质 | 敌手能力限制                       |
| --- | -------------------------------------- | ---- | ---------------------------- |
| 1   | $(\delta_0, \ell_0) \to (\delta_0, 0)$ | 平移隐藏 | 无法区分 $\ell_0$ 与 $0$          |
| 2   | $(\delta_0, 0) \to (\delta_1, 0)$      | 尺度隐藏 | 无法区分 $\delta_0$ 与 $\delta_1$ |
| 3   | $(\delta_1, 0) \to (\delta_1, \ell_1)$ | 平移隐藏 | 无法区分 $0$ 与 $\ell_1$          |

---

### 二、核心总结

| 组件   | 关键内容                                                                      | 密码学意义                               |
| ---- | ------------------------------------------------------------------------- | ----------------------------------- |
| 安全目标 | $\Pi_{\text{aff}}$ 实现 $(\gamma,\eta,\mu)$-参数隐藏                            | 同时隐藏仿射变换的 缩放量 $\delta$ 和 平移量 $\ell$ |
| 技术前提 | $\Pi$ 需满足 $\mathcal{L}$-平滑CLWW泄漏下的模拟安全性                                   | 确保基础方案泄漏可控（仅暴露顺序关系及部分比特位置等值性，非具体值）  |
| 核心方法 | 混合论证 + 等价转换（参数隐藏 $\Leftrightarrow$ 尺度隐藏+平移隐藏）                             | 将复杂安全性拆解为可验证子命题，降低证明难度              |
| 参数约束 | - $\gamma = 2^{\omega(\log \lambda)}$（超多项式）<br> - $D$ 为 $(\eta,\mu)$-平滑分布 | 避免敌手通过小尺度变换或极端分布破解方案                |

