
#### 1. PPH基础定义
 属性保留哈希（PPH） 方案是三元组 $\Gamma = (\mathcal{K}_h, \mathcal{H}, \mathcal{T})$：
- 密钥生成 $\mathcal{K}_h(1^\lambda)$：

	- 随机化算法，生成哈希密钥 $hk$ 和测试密钥 $tk$
	- 隐含定义输入域 $D$ 和输出域 $R$

- 评估算法 $\mathcal{H}(hk, x)$：

	- 随机化算法，输入 $x \in D$，输出哈希值 $h \in R$

- 测试算法 $\mathcal{T}(tk, h_1, h_2)$：

	- 确定性算法，输入两哈希值，输出比特（表示谓词关系）

---
#### 2. 正确性定义（Correctness）

 正确性游戏 $\mathrm{COR}_{\Gamma,P}^{\text{pph}}(\mathcal{A})$：

1. 生成 $(hk, tk) \leftarrow \mathcal{K}_h(1^\lambda)$，$tk$ 给敌手 $\mathcal{A}$
2. $\mathcal{A}$ 提交 $x, y$
3. 计算 $h \leftarrow \mathcal{H}(hk,x)$, $h' \leftarrow \mathcal{H}(hk,y)$
4. 失败条件：若 $\mathcal{T}(tk, h, h') \neq P(x,y)$，输出 $1$ 要求：对所有高效敌手 $\mathcal{A}$，失败概率 $\Pr[\text{输出}1]$ 可忽略（$negl(\lambda)$）

---
**受限选择输入安全游戏 $\mathrm{IND}_{\Gamma,P}^{\text{pph}}(\mathcal{A})$**

| 步骤                      | 操作                                                                                    |
| ----------------------- | ------------------------------------------------------------------------------------- |
| 初始化                     | $(hk, tk) \leftarrow \mathcal{K}_h(1^\lambda)$，发送$tk$秘钥给敌手                            |
| 挑战生成                    | $x^* \leftarrow \mathcal{A}(tk)$,发送$x^*$给挑战者                                          |
| 哈希计算                    | $h_0 \leftarrow \mathcal{H}(hk,x^*)$, $h_1 \leftarrow R$（随机值）                         |
| 挑战响应                    | $b \leftarrow \{0,1\}$, 返回 $h_b$ 给 $\mathcal{A}$                                      |
| 敌手猜测                    | $b' \leftarrow \mathcal{A}^{\text{HASH}}(tk, x^*, h_b)$                               |
| 胜败判定                    | 返回 $(b \overset{?}{=} b')$                                                            |
| 预言机 $\text{HASH}(x)$限制： | - 若 $P(x^*,x)=1$ 或 $P(x,x^*)=1$，返回 $\perp$<br>- 否则返回 $h \leftarrow \mathcal{H}(hk,x)$ |


---
#### PPH-ORE方案构造基础
设函数 $F: K \times ([n] \times \{0,1\}^n) \to \{0,1\}^\lambda$ 是安全的伪随机函数（PRF），谓词 $P(x,y) = \mathbf{1}(x = y + 1)$ 表示仅当 $x$ 比 $y$ 大 $1$ 时输出 $1$。 设 $\Gamma = (\mathcal{K}_h, \mathcal{H}, \mathcal{T})$ 是基于 $P$ 的保性质哈希（PPH） 方案，则ORE方案 $\Pi = (\mathcal{K}, \mathcal{E}, \mathcal{C})$ 定义如下：

 1. **密钥生成 $\mathcal{K}(1^\lambda, M)$：**

- 随机选取 $F$ 的密钥 $k \leftarrow K$；
- 运行 $\Gamma.\mathcal{K}_h$ 生成PPH密钥 $(hk, tk)$；
- 输出比较密钥 $ck \leftarrow tk$，私钥 $sk \leftarrow (k, hk)$。

---

2. **加密 $\mathcal{E}(sk, m)$：**

- 将明文 $m$ 转为二进制 $(b_1,\dots,b_n)$；
- 计算每比特值 $u_i = F(k, (i, b_1\dots b_{i-1}\mathbf{0}^{n-i+1})) + b_i \mod 2^\lambda$；
- 对 $u_i$ 哈希：$t_i = \Gamma.\mathcal{H}(hk, u_i)$；
- 随机置换 $\pi: [n] \to [n]$，生成密文 $CT = (v_1,\dots,v_n) = (t_{\pi(1)},\dots,t_{\pi(n)})$。

3. **比较 $\mathcal{C}(ck, CT_1, CT_2)$：**

- 输入密文 $CT_1 = (v_1,\dots,v_n), CT_2 = (v_1',\dots,v_n')$；
- 遍历 $i,j \in [n]$，运行测试：
- 若存在 $\Gamma.\mathcal{T}(tk, v_i, v_j') = 1$，输出 $1$（$m_1 > m_2$）；
- 若存在 $\Gamma.\mathcal{T}(tk, v_i', v_j) = 1$，输出 $0$（$m_1 < m_2$）；
- 否则输出 $\perp$（$m_1 = m_2$）。

---


#### 1. ORE方案 $\Pi$ 的正确性（Correctness）

正确性逻辑：

- 设明文 $m_1 > m_2$，其二进制分别为 $(b_1,\dots,b_n)$ 和 $(b_1',\dots,b_n')$；
- 存在唯一索引 $i^*$ 使得 $u_{i^*} = u_{i^*}' + 1$（即 $m_1$ 在 $i^*$ 位比 $m_2$ 大 $1$）；
- 比较结果正确性依赖PPH：PPH的测试算法 $\mathcal{T}$ 可检测 $P(u_{i^*}, u_{i^*}')=1$（满足 $x=y+1$），故方案 $\Pi$ 正确输出 $m_1 > m_2$；
- 其他情况同理：当 $m_1 = m_2$ 或 $m_1 < m_2$ 时，通过类似机制保证正确性。

---
#### 2. 安全性定理（Theorem 10）

 若以下条件成立，则 $\Pi$ 满足 $\mathcal{L}_f$-非自适应模拟安全：

- 基础安全假设：

	- $F$ 是安全伪随机函数（PRF）；
	- $\Gamma$（PPH方案）满足受限选择输入安全。


 **$\mathcal{L}_f$-非自适应模拟安全，则$\Pi$ 满足平滑CLWW方案。**

---
#### 3. 混合证明框架（Hybrid Argument）

| 阶段               | 游戏定义                                                    | 核心操作变化                                        |
| ---------------- | ------------------------------------------------------- | --------------------------------------------- |
| $H_{-1}$         | 真实游戏 $\mathrm{REAL}_{\Pi}^{\mathcal{ORE}}(\mathcal{A})$ | 原方案加密                                         |
| $H_0$            | 替换PRF为真随机函数 $F^*$                                       | $F_k(\cdot) \to F^*(\cdot)$                   |
| $H_{i\cdot q+j}$ | 依赖谓词 $\mathrm{Switch}_{(i,j)}$                          | 若 $\mathrm{Switch}_{(i,j)}=1$，替换 $u_i^j$ 为随机串 |

谓词 $\mathrm{Switch}_{(i,j)}$ 定义：

$$\mathrm{Switch}_{(i,j)} = \begin{cases} 1 & \text{若 } \forall k \in [q],\ \mathsf{msdb}(m_j, m_k) \neq i \\ 0 & \text{否则} \end{cases}$$

关键限制：

- 当 $\mathrm{Switch}_{(i,j)}=0$ 时（泄漏位），存在 $u_i^k$ 使得 $u_i^j = u_i^k \pm 1$，PPH可检测此关系；
- 禁止随机化：若替换此类 $u_i^j$ 为随机串，敌手可轻易区分。