---
tags:
  - excalidraw
data: 2024-11-13T21:09:00
width: "2560"
height: "1440"
---
### 翻译：

**非归罪性**。最后，我们要求可追踪秘密共享方案满足非归罪性（non-imputability）的概念，即不能错误地归罪于诚实的参与方。具体来说，这意味着即使是恶意的追踪者也无法生成一个证明来指控一个诚实的参与方有罪，并获得接受。该概念通过图2中的安全实验进行形式化定义。在该实验中，对手获得追踪密钥 $tk$ 和验证密钥 $vk$，并可以获取其选择的任意参与方的秘密共享，但不能是其试图诬陷的诚实参与方的共享。

**定义 6（非归罪性）**。我们称一个可追踪秘密共享方案 $\text{TTSS} = (\text{Share}, \text{Rec}, \text{Trace}, \text{Verify})$ 满足非归罪性，如果对于任意的概率多项式时间对手 $A$，以下函数在 $\lambda$ 上是可忽略的：

$$\text{Adv}^{\text{ni}}_{A, \text{TTSS}}(\lambda) := \Pr \left[\text{ExpNI}_{A, \text{TTSS}}(\lambda) = 1\right]$$