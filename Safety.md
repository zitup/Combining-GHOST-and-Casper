## 安全性

回想⼀下，我们的主要⽬标是证明安全性和活跃性。在本节中，我们证明安全性，其做法与 Casper FFG [[7](/README.md#7)] 类似；主要区别在于我们使⽤时期边界对（⽽不是 Casper 的检查点区块），并且我们的最终确定定义更为复杂。因此，我们需要以下论点，它必然⽐ Casper 中的对应思想更复杂。  
<span id="5.1" style="font-weight: bold">**引理 5.1。** </span> 在视图 $G$ 中，如果 $(B$<sub>$F$</sub>$, f) ∈ F(G)$ 且 $(B$<sub>$J$</sub>$, j) ∈ J(G)$，且 $j > f$，则 $B$<sub>$F$</sub> 必定是 $B$<sub>$J$</sub> 的祖先，或者区块链是 (1/3)-可削减的 - 具体来说，必须存在 $V$ 的 2 个子集 $V1,V2$，每个子集的总权益至少为 $2N/3$，使得它们的交集都违反削减条件（S1）或都违反削减条件（S2）。

_证明。_ 预期⽭盾，假设有⼀对 $(B$<sub>$J$</sub>$, j)$ 且 $j > f$，且 $B$<sub>$J$</sub> 不是 $B$<sub>$F$</sub> 的后代。根据最终性定义，在 $G$ 中，我们必须有 $(B$<sub>$F$</sub>$, f)\overset{J}{\rightarrow}(B$<sub>$k$</sub>$, f+k)$ ，其中我们有⼀系列相邻的时期边界对 $(B$<sub>$F$</sub> $,$ $f),(B$<sub>$1$</sub>$, f + 1), . . . ,(B$<sub>$k$</sub>$, f + k)$。  
由于 $(B$<sub>$J$</sub>$, j)$ 是合理的，且 $B$<sub>$J$</sub> 不是 $B$<sub>$F$</sub> 的后代，在不失⼀般性的情况下（通过绝⼤多数链接向后追溯），我们可以假设 $(B$<sub>$J$</sub>$, j)$ 是最早的此类违规⾏为，这意味着我们可以假设 $(B$<sub>$l$</sub>$, l)\overset{J}{\rightarrow}(B, j)$，其中 $l < f$ 但 $j > f$（这⾥我们使⽤[引理 4.11](/Main-Protocol-Gasper.md#4.11)，它告诉我们没有两个被证明的区块具有相同的 aep()，否则我们已经完成了 2 个权重为 $2N/3$ 的验证器⼦集，每个⼦集都违反了（S1）；这就是我们不必担⼼相等情况的原因）。由于 $B$<sub>$1$</sub>$, . . . , B$<sub>$k$</sub> 都是合理的但是是 $B$<sub>$F$</sub> 的后代，我们知道 $B$<sub>$J$</sub> 不可能是这些区块中的任何⼀个，所以我们必须有 $j > f + k$。这意味着视图 $G$ 看到 $V$ 的某个⼦集 $V1$ 的总权益超过 $2N/3$ 已经做出了证明来证明检查点边 $(B$<sub>$l$</sub>$, l){\rightarrow}(B$<sub>$J$</sub>$, j)$，因此对于任何这样的证明 $α1$， aep(LJ($α1$)) $=l$ 和 ep($α1$) $= j$。类似地， $G$还看到超过 $2N/3$ 权重的验证者 $V2$ 已经做出证明证明 $(B$<sub>$F$</sub>$, f){\rightarrow}(B$<sub>$k$</sub>$, f+k)$，因此对于任何此类证明 $α2$， aep(LJ($α2$)) $=f$ 和 ep($α2$) $= f + k$。因此，对于交集 $V1 ∩ V2$ 中的任何⼈，他们做了两个不同的证明 $α1$ 属于前一种类型， $α2$ 属于后一种类型。因为  
$$l < f < f + k < j,$$
我们知道
$$aep (LJ (α1)) < aep (LJ (α2)) < ep (α2) < ep (α1),$$
这使得它们可以被 (S2) 削减。

我们现在已准备好证明安全性。  
**定理 5.2**（安全性）。在视图 $G$ 中，如果我们不具备以下属性，则 $G$ 是 (1/3)-可削减的。

1. 当视图更新时， $F(G)$ 中的任何对都会保留在 $F(G)$ 中。
2. 若 $(B, j) ∈ F(G)$，则 $B$ 在 $G$ 的规范链中。

_证明。_ 第⼀个属性从 $F(G)$ 和 $J(G$) 的定义中很容易看出，因此我们省略证明。  
[算法 4.2](/Main-Protocol-Gasper.md#algorithm-4.2) 的定义表明，规范链总是经过具有最⾼证明时期 $j$ 的合理对，因此根据[引理 5.1](#5.1)，必须经过 $F(G)$ 中最⾼的最终对。因此，⾜以证明没有两个最终确定的区块发⽣冲突，因为所有最终确定的区块必须位于同⼀条链上，我们刚刚表明这条链必须是规范链的⼦集。  
我们现在证明，如果 $(B$<sub>$1$</sub>$,f$<sub>$1$</sub>$),(B$<sub>$2$</sub>$,f$<sub>$2$</sub>$) ∈ F(G)$ 但是 $B$<sub>$1$</sub> 和 $B$<sub>$2$</sub> 冲突，则 $G$ 是 (1/3)-可削减的。  
不失⼀般性， $f2 > f1$。由于 $(B$<sub>$2$</sub>$, f$<sub>$2$</sub>$)$已最终确定，因此它是合理的，我们可以应⽤[引理 5.1](#5.1)。这表明，要么 $B$<sub>$2$</sub> 必须是 $B$<sub>$1$</sub> 的后代（由于它们冲突，因此假设是不可能的），要么 $G$ 是 (1/3)-可削减的，正如所期望的那样。

最后，为了从定理中获得我们最初的安全概念，请注意，最终确定的区块的定义会⾃动保留在未来视图中。
