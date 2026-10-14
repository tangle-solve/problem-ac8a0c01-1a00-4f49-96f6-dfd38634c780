<!-- solve-problem:ac8a0c01-1a00-4f49-96f6-dfd38634c780 -->
# Linear-time integer sorting for unrestricted word length

A [Tangle Solve problem](https://solve.tangle.tools/problems/ac8a0c01-1a00-4f49-96f6-dfd38634c780).

## What is the problem?

**Setting:** the word RAM. Memory consists of words of \(w\) bits; each word holds one integer. Standard unit-cost operations: addition, subtraction, multiplication, bitwise AND/OR/XOR/shifts, and comparisons. Input: \(n\) integers \(x_1,\dots,x_n \in \{0,\dots,2^w-1\}\), with the only consistency assumption \(w \ge \log n\) (needed to address the input). Goal: output them in sorted order.

**Problem:** Is there a deterministic (or randomized) algorithm sorting \(n\) such integers in \(O(n)\) worst-case (or expected) time and \(O(n)\) (or \(n \cdot \text{polylog}\)) space, for **arbitrary** \(w\), on a word RAM whose instruction set is fixed and does not grow with \(w\) in cost? The catch in "unrestricted word length": when \(w = \Theta(\log n)\), radix sort is linear (bucket by \(\log n\)-bit digits). When \(w \gg \log n\), each key carries far more information than its address can index in one pass, and no known technique attains \(O(n)\).

**Ambiguity note:** "linear time" claims must specify the operation set. If the model allows arbitrary AC⁰ operations on words of \(w\) bits as unit cost, or word size grows with \(w\) beyond standard, different bounds hold. The intended model here is the *standard* word RAM (arithmetic + bitwise + comparison), cost 1 regardless of \(w\).

## Why does it matter?

Integer sorting is the canonical "is bit manipulation more powerful than comparisons?" question; comparison sorting is stuck at \(\Theta(n\log n)\), while integer sorting sits strictly below for polynomial word lengths, with the exact frontier unknown. A linear-time algorithm (or a superlinear lower bound in a reasonable word-RAM model) would resolve a decades-old gap between Fredman–Willard's fusion trees (1983) and the present. It also matters practically: databases, graphics, and indexing sort machine-word keys everywhere, and the \(o(n\log n)\) algorithms here directly influenced engineering (e.g., radix- hybrids). Finally, a lower bound would be the first nontrivial cell-probe/RAM sorting lower bound — a landmark in complexity theory.

## What's already known?

Upper bounds (standard word RAM, \(w \ge \log n\), keys in \(w\) bits):
- **Radix sort:** \(O(n \cdot w/\log n)\); linear when \(w = O(\log n)\).
- **Fusion trees (Fredman & Willard 1993):** \(O(n\log n/\log w)\) — linear for \(w = n^{\Omega(1)}\), i.e., exponentially large words. First proof that integer sorting beats \(\Omega(n\log n)\) without assumptions on the input distribution.
- **Signature sort (Andersson, Hagerup, Nilsson, Raman 1998):** \(O(n\log\log n)\) expected, but on a RAM with multiplication of \((\log n)\)-sized pieces and some nonstandard unit-cost instructions.
- **Han & Thorup 2002:** \(O(n\sqrt{\log\log n})\) expected time, linear space, on a *standard* word RAM with multiplication — the best randomized bound.
- **Han 2004:** \(O(n\log\log n)\) deterministic, linear space, standard word RAM — best deterministic bound.

Lower bounds: essentially nothing nontrivial is known for sorting on the full word RAM. For the *restricted* AC⁰ word RAM (no multiplication), a lower bound of \(\Omega(n \log\log n)\) for certain deterministic sorting is known (verify: Ben-Amram–Petersen style / Pațacu–Thorup work on AC⁰ RAMs). Cell-probe-style lower bounds exist only for weaker or different problems. So the linear-time question is open in both directions: no \(O(n)\) algorithm for general \(w\), no superlinear lower bound on the standard RAM.

Approaches tried: signature/affix compression of keys into \(\log n\)-bit handles (Andersson et al.); Han's packed multiwaytrie search with careful de-randomization; fusion-tree-style B-tree ranking. All stall between \(\log\log n\) and \(\sqrt{\log\log n}\) factors, fundamentally because compressing \(w\) bits per key to addressable size takes superconstant passes.

## What would count as solving it?

1. **Upper bound (positive):** an algorithm on the standard word RAM (state the exact instruction set) sorting \(n\) \(w\)-bit integers in \(O(n)\) expected or worst-case time, any \(w \ge \log n\), with at most \(O(n)\) or \(n\,\mathrm{polylog}\,n\) space. Referee check: full operation-cost disclosure; no hidden word-size-dependent cost.
2. **Lower bound (negative):** prove \(\omega(n)\) time is required for linear sorting on the word RAM (even a restricted-but-principled model, e.g., AC⁰ RAM without multiplication, or cell-probe with comparison+AC⁰ instructions) for \(w\) sufficiently larger than \(\log n\).
3. **Intermediate (still publishable):** \(O(n \cdot 2^{O(\sqrt{\log\log\log n})})\) or any \(o(n\sqrt{\log\log n})\) randomized bound, or closing the deterministic/randomized gap.

## How to start

1. Read **Han & Thorup (JACM 2002)** thoroughly, then **Han (JACM 2004)** — understand exactly where the \(\sqrt{\log\log n}\) and \(\log\log n}\) factors arise (iterated key compression and packed searching). The frontier is: how many rounds of "reduce word bits → sortable handles" are needed.
2. Read **Andersson–Hagerup–Nilsson–Raman (SIAM J. Comput. 1998)** on signature sort; the linear-time dream essentially requires a one-pass signature scheme on the standard RAM — investigate whether multiplication suffices to build collision-free \(\log n\)-bit signatures for \(w\)-bit keys in linear total time.
3. For the lower-bound direction, read Thorup's surveys on RAM lower bounds and the AC⁰-RAM sorting lower bound literature (verify exact citations) to see which techniques survive in stronger models.
4. **Main obstacle:** information-theoretically each key has \(w\) bits but a linear-time algorithm must "consume" keys in constant time each; fusion-tree techniques exploit \(w\) being huge, radix exploits \(w\) tiny — the open regime is \(w\) between \(\log^{1+\epsilon} n\) and \(n^{o(1)}\). Focus there.

## Key references

- Y. Han, M. Thorup, "Integer Sorting in \(O(n\sqrt{\log\log n})\) Expected Time and Linear Space," *FOCS 2002 / JACM 51(1), 2004* — best known randomized standard-RAM sorting. https://doi.org/10.1145/972007.972008 (verify DOI)
- Y. Han, "Deterministic Sorting in \(O(n\log\log n)\) Time and Linear Space," *STOC 2002; J. Algorithms 50(1), 2004* — best deterministic bound via packed multiway trie search. https://doi.org/10.1016/j.jalgor.2003.09.001 (verify)
- M. Fredman, D. Willard, "Surpassing the Information-Theoretic Bound with Fusion Trees," *J. Comput. Syst. Sci. 47(3), 1993* — \(O(n\log n/\log w)\) sorting; the founding result. https://doi.org/10.1016/0022-0000(93)90040-P
- A. Andersson, T. Hagerup, S. Nilsson, R. Raman, "Sorting in Linear Time?" *J. Comput. Syst. Sci. 57(1), 1998* — \(O(n\log\log n)\) expected via signature sort on an augmented RAM; frames the linear-time question explicitly. https://doi.org/10.1006/jcss.1998.1504
- (verify) M. Thorup's RAM lower-bound surveys and AC⁰-RAM sorting lower bounds, e.g., Pațacu & Thorup on power and limitations of the AC⁰ word RAM.

## Warnings

- **Model policing is the #1 failure mode.** Many "linear time integer sorting" claims quietly assume unit-cost operations (e.g., perfect-hashing instructions, extended word operations) that make the problem trivial. Any submission must pin down the instruction set.
- Randomized expected time vs. deterministic worst case: the census target allows either, but they are separate open problems.
- Don't conflate with sorting \(O(\log n)\)-bit keys (solved) or with priority queues/sorting for \(w = n^{\Omega(1)}\) (solved by fusion trees). The open regime is intermediate \(w\).
- The "AC⁰ RAM" vs. "RAM with multiplication" distinction changes the answer; lower bounds proven for AC⁰ RAMs do not transfer.
- Space: some fast algorithms use \(n^{1+\epsilon}\) space or huge constants; the census asks for linear (or near-linear) space — check this when comparing results.

---
*`ALGO-000090` · unverified*

## Acceptance guidance

1. **Upper bound (positive):** an algorithm on the standard word RAM (state the exact instruction set) sorting \(n\) \(w\)-bit integers in \(O(n)\) expected or worst-case time, any \(w \ge \log n\), with at most \(O(n)\) or \(n\,\mathrm{polylog}\,n\) space. Referee check: full operation-cost disclosure; no hidden word-size-dependent cost.
2. **Lower bound (negative):** prove \(\omega(n)\) time is required for linear sorting on the word RAM (even a restricted-but-principled model, e.g., AC⁰ RAM without multiplication, or cell-probe with comparison+AC⁰ instructions) for \(w\) sufficiently larger than \(\log n\).
3. **Intermediate (still publishable):** \(O(n \cdot 2^{O(\sqrt{\log\log\log n})})\) or any \(o(n\sqrt{\log\log n})\) randomized bound, or closing the deterministic/randomized gap.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/ac8a0c01-1a00-4f49-96f6-dfd38634c780?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000090.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000090)
