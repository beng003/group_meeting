#### 1. ORE.Setup（密钥生成）

- 输入安全参数 $\lambda \in \mathbb{N}$,选择伪随机函数 $F$ 的均匀随机密钥 $k$,输出私钥 $\mathbf{sk} = k$
#### 2. ORE.Encrypt（加密）

- 输入私钥 $\mathbf{sk} = k$、明文 $m$,将 $m$ 转换为 $d$ 进制表示 $b_1 b_2 \cdots b_n$,对每个位置 $i \in [n]$，计算：$u_i = F \Big( k, \, \big( i, \, b_1 b_2 \cdots b_{i-1} \Vert 0^{n-i} \big) \Big) + b_i \pmod{M}$, 输出密文 $\mathbf{ct} = (u_1, u_2, \ldots, u_n)$
#### 3. ORE.Compare（密文比较）

- 输入两个密文 $\mathbf{ct}_1 = (u_1, \ldots, u_n)$, $\mathbf{ct}_2 = (u'_1, \ldots, u'_n)$,寻找最小索引 $i$ 使得 $u_i \neq u'_i$, 若不存在 $i$，输出 $0$（明文相等）;若存在 $i$，检查是否 $\exists \, k \in [d-1]$ 满足：$u'_i = u_i + k \pmod{M}$,成立则输出 $1$ $( m_1 < m_2 )$,否则输出 $0$（$m_1 > m_2$）