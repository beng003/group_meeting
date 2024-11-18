---
theme: gaia
_class: lead
paginate: true
backgroundColor: "#fff"
---
# **Traceable Secret Sharing: Strong Security and Efficient Constructions**

Dan Boneh, Aditi Partap, and Lior Rotem
Stanford University

https://marp.app/

---


## **How to write slides**

1. 111
2. 222
3. 333
Split pages by horizontal ruler (`---`). It's very simple! :satisfied:

```markdown
# Slide 1

foobar

---

# Slide 2

foobar
```

$1$

---

1111
你好

---

<style scoped>
section h1 {text-align: center;font-size: 80px;color:black;}
footer{color:black;font-size: 20px;} 
</style>
<!-- _class: lead gaia -->
## 1. **分发机制**

---


设秘密为一个 $d$-维空间中的点 $S = (x_1, x_2, \dots, x_d)$，$S$ 是要共享的秘密点坐标。

1. **构造几何超平面**：
   - 在 $d$-维空间中，一个超平面可以表示为：
     $$a_1x_1 + a_2x_2 + \dots + a_dx_d = b$$
     其中 $a_1, a_2, \dots, a_d$ 是超平面的系数，$b$ 是常数项。
   - 选择 $n \geq t$ 个超平面，其中 $t$ 是门限值。每个超平面用一组随机的系数 $a_1, a_2, \dots, a_d$ 和常数项 $b$ 确定。

---

假设 $R$ 中硬编码的份额是 $(x_i, y_i = q(x_i))$，其中 $i = 1, \ldots, t-1$。当给 $R$ 一个额外的形式为 $(x_t, y_t)$ 的份额时，$R$ 需要输出：

$$s = \sum_{i \in [t]} \left( \prod_{k \in [t] \setminus \{i\}} \frac{x_k}{x_k - x_i} \right) \cdot y_i$$

否则，$R$ 就不是一个良好的重建盒。该公式可以重新写成：

$$s = \sum_{i \in [t-1]} \left( \prod_{k \in [t] \setminus \{i\}} \frac{x_k}{x_k - x_i} \right) \cdot y_i + \left( \prod_{k \in [t-1]} \frac{x_k}{x_k - x_t} \right) \cdot y_t \tag{1}$$

如果我们将形式为 $(x_t, y_t + 1)$ 的份额提供给 $R$，其中 $x_t$ 和 $y_t$ 保持不变，则得到另一个 $s'$ 满足：

$$s' = \sum_{i \in [t-1]} \left( \prod_{k \in [t] \setminus \{i\}} \frac{x_k}{x_k - x_i} \right) \cdot y_i + \left( \prod_{k \in [t-1]} \frac{x_k}{x_k - x_t} \right) \cdot (y_t + 1) \tag{2}$$

将公式 (1) 从公式 (2) 中相减并重排，得到：

$$\prod_{k \in [t-1]} \frac{x_k - x_t}{x_k} = (s' - s)^{-1} \tag{3}$$
---
<style scoped>
section h1 {text-align: center;font-size: 80px;color:black;}
footer{color:black;font-size: 20px;} 
</style>
<!-- _class: lead gaia -->

# Marp for THU slides