---
marp: true
theme: gaia
paginate: true
footer: 
style: "section footer{color:black;font-size: 20px;}"
---
<style scoped>
<style scoped>
section h1 {text-align: center;font-size: 60px;color:black;}
section {
  background-image:url('./fm.png');
  background-size:cover
}
footer{color:black;font-size: 20px;} 
</style>
<!-- _class: lead gaia -->

# Parameter-Hiding Order Revealing Encryption

David Cash, Feng-Hao Liu, Adam O’Neill, Mark Zhandry,
and Cong Zhang

Asiacrypt 2018


---
<style >

section{
  background-image:url('./bg.png');
  background-size:cover;
  position: absolute;
  }
section h1 {font-size:50px;color:black;margin-top:px;}
section h2 {font-size:40px;color:black;margin-top:px;}
section p {font-size: 35px;color:black;}
section table {text-align: center;font-size: 32px;color:black;}
section a {font-size: 25px;color:black;}
li {font-size: 35px;text-align: left;}

img {
    margin-left: auto; 
    margin-right:auto; 
    display:block;
    margin:0 auto;
    width:25cm;
    }
</style>

<style scoped>
section h1 {font-size:60px;color:black;margin-top:px;}
li {font-size: 30px;text-align: left;}
</style>

# 总览
1. 应用场景
2. 揭序加密定义
3. CLWW方案
4. 平滑泄漏剖面ORE构造参数隐藏ORE
5. PPH构造平滑泄露剖面ORE
6. 双线性映射构造PPH

<style scoped>
li {font-size: 40px;text-align: left;}
</style>

---
# 1. 应用场景

某大学拟外包其学生绩点(GPA)数据库，为简化模型，假设每个学生的学术能力相互独立，且该特性已反映于其GPA数据中，即所有GPA样本均独立同分布于某潜在概率分布。该校显然需要对学生个体GPA保密，同时可能要求隐藏诸如平均值、方差等聚合统计量以避免因此获得评分过严或过松的名声。

<style scoped>
section p {font-size: 45px;color:black; text-indent: 2em; line-height: 1.5;}
</style>

---
- **攻击背景​**​：**传统 ORE 存在的严重安全缺陷**，特别是在攻击者具备数据分布知识的情况下防护性不足。
- **研究场景设定 (数据库条目均值与方差未知)​**​：本研究聚焦的特定场景。关键点是：
    - ​**独立同分布 (i.i.d.)​**​：数据生成方式假设。
    - ​**分布形态已知**​：攻击者知道数据属于哪类分布（如：正态分布）。
    - ​**均值与方差未知**​：分布的关键参数对攻击者和数据库（用户）都保密。正是**参数未知性**使得之前的攻击失效，并成为新方案保护的要点。

---

# 2. 揭序加密定义
揭序加密（ORE）方案是一个由三个算法构成的元组 $\Pi = (\mathcal{K}, \mathcal{E}, \mathcal{C})$：

- **密钥生成算法 $\mathcal{K}$：**
	- 性质：随机化算法
	- 输入：安全参数 $\lambda$ 的二进制表示 $1^\lambda$ 和明文空间上限 $M$
	- 输出：始终生成两个密钥——加密密钥 sk 和 比较密钥 ck。

- **加密算法 $\mathcal{E}$：**
	- 性质：随机化算法
	- 输入：秘密密钥 sk 和明文 $m$，其中 $m \in [M] = \{0,1,\dots,M-1\}$
	- 输出：单个密文 $c$。

<style scoped>
section h1 {font-size:60px;color:black;margin-top:px;}
li {font-size: 30px;text-align: left;}
</style>

---
- **比较算法 $\mathcal{C}$：**
	- 性质：确定性算法
	- 输入：比较密钥 ck 和两个密文 $c_1, c_2$
	- 输出：一个比特位（表示比较结果）。



特殊情形： 若比较算法 $\mathcal{C}$ 是简单整数比较（即：将密文 $c_1, c_2$ 解释为整数的二进制表示，并直接比较大小），则该方案称为保序加密（OPE）方案。


<style scoped>
section p {font-size: 45px;color:black; text-indent: 0em; line-height: 1;}
</style>

---
# 3. CLWW方案
- ORE.Setup：输入安全参数 $\lambda \in \mathbb{N}$,选择伪随机函数 $F$ 的均匀随机密钥 $k$,输出私钥 $\mathbf{sk} = k$
- ORE.Encrypt：输入私钥 $\mathbf{sk} = k$、明文 $m$,将 $m$ 转换为 $d$ 进制表示 $b_1 b_2 \cdots b_n$,对每个位置 $i \in [n]$，计算$u_i = F \Big( k, \, \big( i, \, b_1 b_2 \cdots b_{i-1} \Vert 0^{n-i} \big) \Big)+ b_i \pmod{M}$, 输出密文 $\mathbf{ct} = (u_1, u_2, \ldots, u_n)$
- ORE.Compare：输入两个密文 $\mathbf{ct}_1 = (u_1, \ldots, u_n)$, $\mathbf{ct}_2 = (u'_1, \ldots, u'_n)$,寻找最小索引 $i$ 使得 $u_i \neq u'_i$, 若不存在 $i$，输出 $0$（明文相等）;若存在 $i$，检查是否 $\exists \, k \in [d-1]$ 满足：$u'_i = u_i + k \pmod{M}$,成立则输出 $1$ $( m_1 < m_2 )$,否则输出 $0$（$m_1 > m_2$）

<style scoped>
section p {font-size: 25px;color:black; text-indent: 0em; line-height: 1;}
li {font-size: 30px;text-align: left;}
</style>

---
# 4. 平滑泄漏剖面ORE构造参数隐藏ORE
## 4.1 平滑CLWW泄漏剖面
非自适应泄漏剖面 $\mathcal{L}_f$ 的定义： 该泄漏剖面以消息向量 $\boldsymbol{m}=(m_1,\ldots,m_q)$ 为输入，输出以下信息：

$$\mathcal{L}_f(m_1,\ldots,m_q) := \Big( \forall 1\leq i,j,k\leq q,\ \ 1(m_i < m_j),\ 1\big(\mathsf{msdb}(m_i,m_j) = \mathsf{msdb}(m_i,m_k)\big) \Big)$$

- $\mathcal{L}_f$ 的核心特性：
	- 泄漏量严格小于CLWW($\mathsf{msdb}(m_i,m_j)$表示首位不同的位置)：$\mathcal{L}_\mathrm{clww}(m_1,\ldots,m_q):=(\forall1\leq i,j\leq n,\mathbf{1}(m_i<m_j),\mathsf{msdb}(m_i,m_j))$
	- 左移不变性（平滑性）：若将所有明文左移一位（即 $m_i \leftarrow 2m_i$），泄漏结果不变：$\mathcal{L}_f(\boldsymbol{m}) = \mathcal{L}_f(2\boldsymbol{m})$

<style scoped>
section p {font-size: 30px;color:black; text-indent: 0em; line-height: 1;}
li {font-size: 33px;text-align: left;}
</style>


---
# 4. 平滑泄漏剖面ORE构造参数隐藏ORE
## 4.2  参数隐藏揭序加密


预先设定参数：$q=\text{poly}(\lambda), \quad M=2^{\text{poly}(\lambda)}, \quad \gamma=2^{\omega(\log\lambda)}, \quad \eta,\mu\leq O(1)$
假设$\gamma$和$M$经向上取整后为精确的$2$的幂。定义：  
$\tau=\gamma, \quad \xi=\gamma^{2}, \quad U=4\xi M, \quad T=\gamma^{2}\times U, \quad K=2\times T$
 设$\Pi=(\mathcal{K},\mathcal{E},\mathcal{C})$为消息空间$[K]$上具有**平滑化CLWW泄露**$\mathcal{L}$的ORE方案。定义消息空间$[M]$上的新ORE方案$\Pi_{\text{aff}}=(\mathcal{K}_{\text{aff}},\mathcal{E}_{\text{aff}},\mathcal{C}_{\text{aff}})$如下：

<style scoped>
section p {font-size: 35px;color:black; text-indent: 0em; line-height: 2;}
</style>

---

- $\mathcal{K}_{\text{aff}}(1^{\lambda},M,\Pi)$：  
  - 输入安全参数$\lambda$、消息空间$[M]$和$\Pi$。选取超多项式全局参数$\gamma=2^{\omega(\log\lambda)}$，计算上述参数。运行$(ck,sk)\leftarrow\mathcal{K}(1^{\lambda},K)$，从$\log U(\xi,2\xi-1)$抽取$\alpha$，从$[T]'$的离散均匀分布抽取$\beta$。输出$sk_{\text{aff}}=(sk,\alpha,\beta)$和$ck_{\text{aff}}=ck$。  
- $\mathcal{E}_{\text{aff}}(sk_{\text{aff}},m)$：  
  - 输入密钥$sk_{\text{aff}}$和消息$m\in[M]$，输出密文：
  - $CT_{\text{aff}}=\mathcal{E}(\alpha m + \beta)$（基于$\Pi$的消息空间$[K]$设计，输入必在有效域内）。  

- $\mathcal{C}_{\text{aff}}(ck_{\text{aff}},CT_{\text{aff}}^{0},CT_{\text{aff}}^{1})$：  
  - 输入比较密钥$ck_{\text{aff}}$和两个密文，输出$\mathcal{C}(ck_{\text{aff}},CT_{\text{aff}}^{0},CT_{\text{aff}}^{1})$。

---

$\Pi_{\text{scale}}=(\mathcal{K}_{\text{scale}},\mathcal{E}_{\text{scale}},\mathcal{C}_{\text{scale}})$**（缩放隐藏）**：  
  - **密钥生成**：类似$\Pi_{\text{aff}}$，但仅抽取$\alpha \stackrel{\$}{\leftarrow} \log U(\xi,2\xi-1)$，输出$sk_{\text{scale}}=(sk,\alpha)$。  
  - **加密**：$CT_{\text{scale}} = \mathcal{E}(\alpha m)$。  
  - **比较**：直接调用$\Pi$的比较算法。
  
$\Pi_{\text{shift}}=(\mathcal{K}_{\text{shift}},\mathcal{E}_{\text{shift}},\mathcal{C}_{\text{shift}})$**（平移隐藏）**：  
  - **密钥生成**：抽取离散均匀分布$\beta \leftarrow [T]'$，输出$sk_{\text{shift}}=(sk,\beta)$。  
  - **加密**：$CT_{\text{shift}} = \mathcal{E}(m + \beta)$。  
  - **比较**：直接调用$\Pi$的比较算法。


---
## 4.3 安全性证明

**定理7** 若 ORE 方案 $\Pi$ 满足 $\mathcal{L}$-模拟安全性（其中 $\mathcal{L}$ 为平滑 CLWW 泄漏剖面），则对于任意 超多项式参数 $\gamma = 2^{\omega(\log\lambda)}$，方案 $\Pi_{\text{aff}}$ 是 $(\gamma,\eta,\mu)$-参数隐藏的。

- $\gamma$ 约束：缩放参数需足够大（$\gamma$ 超多项式增长，如 $\gamma > 2^{\lambda}$）
- $\mathcal{L}$ 要求：泄漏剖面需满足平滑CLWW性质（抵抗比特级分析攻击）

| 步骤  | 参数过渡                                   | 依赖性质 | 敌手能力限制                       |
| --- | -------------------------------------- | ---- | ---------------------------- |
| 1   | $(\delta_0, \ell_0) \to (\delta_0, 0)$ | 平移隐藏 | 无法区分 $\ell_0$ 与 $0$          |
| 2   | $(\delta_0, 0) \to (\delta_1, 0)$      | 尺度隐藏 | 无法区分 $\delta_0$ 与 $\delta_1$ |
| 3   | $(\delta_1, 0) \to (\delta_1, \ell_1)$ | 平移隐藏 | 无法区分 $0$ 与 $\ell_1$          |


<style scoped>
section li {font-size: 30px;color:black; text-indent: 0em; line-height: 1;}
</style>

---
# 5. PPH构造平滑泄露剖面ORE
## 5.1 基于$P(x_1,x_2)$的属性保持哈希PPH定义$\Gamma = (\mathcal{K}_h, \mathcal{H}, \mathcal{T})$
- 密钥生成 $\mathcal{K}_h(1^\lambda)$：

	- 随机化算法，生成哈希密钥 $hk$ 和测试密钥 $tk$
	- 隐含定义输入域 $D$ 和输出域 $R$

- 哈希算法 $\mathcal{H}(hk, x)$：

	- 随机化算法，输入 $x \in D$，输出哈希值 $h \in R$

- 测试算法 $\mathcal{T}(tk, h_1, h_2)$：

	- 确定性算法，输入两哈希值，输出比特，表示谓词关系$P(x_1,x_2)$

---
# 5. PPH构造平滑泄露剖面ORE
## 5.2 PPH-ORE方案构造

设函数 $F: K \times ([n] \times \{0,1\}^n) \to \{0,1\}^\lambda$ 是安全的伪随机函数（PRF），谓词 $P(x,y) = \mathbf{1}(x = y + 1)$ 表示仅当 $x$ 比 $y$ 大 $1$ 时输出 $1$。 设 $\Gamma = (\mathcal{K}_h, \mathcal{H}, \mathcal{T})$ 是基于 $P$ 的保性质哈希（PPH） 方案，则ORE方案 $\Pi = (\mathcal{K}, \mathcal{E}, \mathcal{C})$ 定义如下：

 - **密钥生成 $\mathcal{K}(1^\lambda, M)$：**

	- 随机选取 $F$ 的密钥 $k \leftarrow K$；
	- 运行 $\Gamma.\mathcal{K}_h$ 生成PPH密钥 $(hk, tk)$；
	- 输出比较密钥 $ck \leftarrow tk$，私钥 $sk \leftarrow (k, hk)$。

---

-  **加密 $\mathcal{E}(sk, m)$：**

	- 将明文 $m$ 转为二进制 $(b_1,\dots,b_n)$；
	- 计算每比特值 $u_i = F(k, (i, b_1\dots b_{i-1}\mathbf{0}^{n-i+1})) + b_i \mod 2^\lambda$；
	- 对 $u_i$ 哈希：$t_i = \Gamma.\mathcal{H}(hk, u_i)$；
	- 随机置换 $\pi: [n] \to [n]$，$CT = (v_1,\dots,v_n) = (t_{\pi(1)},\dots,t_{\pi(n)})$。

-  **比较 $\mathcal{C}(ck, CT_1, CT_2)$：**

	- 输入密文 $CT_1 = (v_1,\dots,v_n), CT_2 = (v_1',\dots,v_n')$；
	- 遍历 $i,j \in [n]$，运行测试：若存在 $\Gamma.\mathcal{T}(tk, v_i, v_j') = 1$，输出 $1$（$m_1 > m_2$）；若存在 $\Gamma.\mathcal{T}(tk, v_i', v_j) = 1$，输出 $0$（$m_1 < m_2$）；
	- 否则输出 $\perp$（$m_1 = m_2$）。

<style scoped>
li {font-size: 30px;text-align: left;}
</style>

---

## 5.3 安全性证明
 若以下条件成立，则 $\Pi$ 满足 $\mathcal{L}_f$-非自适应模拟安全：
- $F$ 是安全伪随机函数（PRF）；
- $\Gamma$（PPH方案）满足受限选择输入安全。


 **$\mathcal{L}_f$-非自适应模拟安全，则$\Pi$ 满足平滑CLWW方案。**


| 阶段               | 游戏定义                                                    | 核心操作变化                                        |
| ---------------- | ------------------------------------------------------- | --------------------------------------------- |
| $H_{-1}$         | 真实游戏 $\mathrm{REAL}_{\Pi}^{\mathcal{ORE}}(\mathcal{A})$ | 原方案加密                                         |
| $H_0$            | 替换PRF为真随机函数 $F^*$                                       | $F_k(\cdot) \to F^*(\cdot)$                   |
| $H_{i\cdot q+j}$ | 依赖谓词 $\mathrm{Switch}_{(i,j)}$                          | 若 $\mathrm{Switch}_{(i,j)}=1$，替换 $u_i^j$ 为随机串 |
<style scoped>
li {font-size: 30px;text-align: left;}
</style>

---
谓词 $\mathrm{Switch}_{(i,j)}$ 定义：

$$\mathrm{Switch}_{(i,j)} = \begin{cases} 1 & \text{若 } \forall k \in [q],\ \mathsf{msdb}(m_j, m_k) \neq i \\ 0 & \text{否则} \end{cases}$$

关键限制：

- 当 $\mathrm{Switch}_{(i,j)}=0$ 时（泄漏位），存在 $u_i^k$ 使得 $u_i^j = u_i^k \pm 1$，PPH可检测此关系；
- 禁止随机化：若替换此类 $u_i^j$ 为随机串，敌手可轻易区分。

---
# 6. 双线性映射构造PPH
## 6.1 双线性映射

**双线性映射**定义了三个素数$p$阶群乘法循环群$G_1，G_2,和G_T$。并且定义在这三个群上的一个映射关系$e: G_1\times G_2 \rightarrow G_T$，并且满足以下的性质：

1. 双线性：对于任意的$g_1 \in G_1, g_2 \in G_2, a,b \in Z_p$，均有$e(g_1^a,g_2^b)=e(g_1,g_2)^{ab}$成立；  
2. 非退化性：$\exists g_1 \in G_1, g_2 \in G_2满足e(g_1,g_2) \neq 1_{G_T}$。
3. 可计算性：存在有效的算法，对于$\forall g_1 \in G_1, g_2 \in G_2$，均可计算$e(g_1,g_2)$。

如果$G_1 = G_2$则称上述双线性配对是对称的，否则是非对称的。


---
##  6.2 属性保持哈希构造
 
 PPH方案 $\Lambda = (\mathcal{K}_h, \mathcal{H}, \mathcal{T})$ ：
**密钥生成 ($\mathcal{K}_h$)：** 输入安全参数 $1^\lambda$，输出：
- 采样素数阶 $p$ 群 $\mathbb{G}, \hat{\mathbb{G}}, \mathbb{G}_T$，生成元 $g \in \mathbb{G}$, $\hat{g} \in \hat{\mathbb{G}}$，双线性映射 $e: \mathbb{G} \times \hat{\mathbb{G}} \rightarrow \mathbb{G}_T$。
- 随机选择 $k \leftarrow \{0,1\}^\lambda$。
- 设哈希密钥 $\mathbf{hk} = (k, g, \hat{g})$，测试密钥 $\mathbf{tk} = (\mathbb{G}, \hat{\mathbb{G}}, \mathbb{G}_T, e)$。

---
**哈希算法 ($\mathcal{H}$)：** 输入 $\mathbf{hk}$ 和 $x \in \{0,1\}^\lambda$：
- 随机选取非零元 $r_1, r_2 \in \mathbb{Z}_p$
- 输出哈希值,其中$F: \{0,1\}^\lambda \times \{0,1\}^\lambda \rightarrow \mathbb{Z}_p$ 是一个伪随机函数（PRF）：$\mathcal{H}(\mathbf{hk}, x) = \left( g^{r_1},\ g^{r_1 \cdot F(k,x)},\ \hat{g}^{r_2},\ \hat{g}^{r_2 \cdot F(k,x+1)} \right)$

**测试算法 ($\mathcal{T}$)**： 输入 $\mathbf{tk}$ 和两组哈希值 $(A_1, A_2, B_1, B_2)$, $(C_1, C_2, D_1, D_2)$：
- 当且仅当下式成立时输出 1：$e(A_1, D_2) = e(A_2, D_1)$
- 否则输出 0。

定义域与值域： 输入空间 $\mathcal{D} = \{0,1\}^\lambda$，输出空间 $\mathcal{R} = \mathbb{G}^2 \times \hat{\mathbb{G}}^2$.

---

## 6.3 安全性证明

在以下双重假设下，PPH（受限选择输入安全）方案可证安全：

1. $F$ 是 PRF；
2. 对称外部 Diffie-Hellman (SXDH) 假设 成立（定义如下）。


 **定义 13 (SXDH 假设)**
设 $\mathbb{G}, \hat{\mathbb{G}}, \mathbb{G}_T$ 为素数阶 $p$ 群，$g$ 与 $\hat{g}$ 分别为 $\mathbb{G}$ 和 $\hat{\mathbb{G}}$ 的生成元，$e: \mathbb{G} \times \hat{\mathbb{G}} \to \mathbb{G}_T$ 是双线性配对。称 SXDH 假设对上述群结构与配对成立，当且仅当对所有高效算法 $\mathcal{A}$：

---
$$\left| \Pr\left[\mathcal{A}(g, g^a, g^b, g^{ab}) = 1\right] - \Pr\left[\mathcal{A}(g, g^a, g^b, T) = 1\right] \right|$$

与

$$\left| \Pr\left[\mathcal{A}(\hat{g}, \hat{g}^a, \hat{g}^b, \hat{g}^{ab}) = 1\right] - \Pr\left[\mathcal{A}(\hat{g}, \hat{g}^a, \hat{g}^b, T) = 1\right] \right|$$

均为关于安全参数 $\lambda$ 的可忽略函数。其中：

- $a, b \xleftarrow{\$} \mathbb{Z}_p$（均匀随机选取）
- $T \xleftarrow{\$} \mathbb{G}_T$（均匀随机选取）

---

采用混合论证法（hybrid argument）。在真实游戏 $H_0 = \mathrm{IND}_{\Gamma,P}^{\text{pph}}(\mathcal{A})$ 中，敌手获得的挑战哈希值为 $(A_1, A_2, B_1, B_2) \in \mathbb{G}^2 \times \hat{\mathbb{G}}^2$。现定义以下混合实验（其中 $R \stackrel{\$}{\leftarrow} \mathbb{G}$与$\hat{R} \stackrel{\$}{\leftarrow} \hat{\mathbb{G}}$ 为独立均匀随机元素）：

1. 混合实验 $\boldsymbol{H_1}$

	- 初始化时，采样均匀随机函数 $F^* \stackrel{R}{\leftarrow} \text{Funs}[\{0,1\}^\lambda, \{0,1\}^\lambda]$ 替代 PRF 密钥 $K$
	- 其余流程与 $H_0$ 一致

2. 混合实验 $\boldsymbol{H_2}$： 挑战哈希值替换为 $(A_1, R, B_1, B_2)$（其中 $R$ 为 $\mathbb{G}$ 中随机元素）
<style scoped>
li {font-size: 30px;text-align: left;}
</style>


---

3.混合实验 $\boldsymbol{H_3}$：挑战哈希值替换为 $(A_1, R, B_1, \hat{R})$（其中 $\hat{R}$ 为 $\hat{\mathbb{G}}$ 中随机元素）

在 $H_3$ 中，敌手实际获得来自值域 $\mathcal{R}$ 的随机元组。由此可得敌手优势：
$$\boxed{ \operatorname{Adv}_{\Gamma,P,\mathcal{A}}^{\text{pph}}(\lambda) = \left| \Pr[H_0 = 1] - \Pr[H_3 = 1] \right| }$$

 **证明逻辑核心：通过构建 $H_0 \rightarrow H_1 \rightarrow H_2 \rightarrow H_3$ 的混合链，将 PPH 安全性归约至 ① PRF 安全 ② SXDH 假设。当 $H_3$ 输出纯随机值时，敌手优势应为 0，故初始优势可忽略。**

---
<style scoped>
section h1 {text-align: center;font-size: 100px;color:black;}
section {
  background-image:url('./fm.png');
  background-size:cover
}
footer{color:black;font-size: 20px;} 
</style>
<!-- _class: lead gaia -->

# 感谢聆听