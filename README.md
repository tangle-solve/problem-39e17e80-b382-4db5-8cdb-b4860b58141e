<!-- solve-problem:39e17e80-b382-4db5-8cdb-b4860b58141e -->
# Splay-tree deque conjecture

A [Tangle Solve problem](https://solve.tangle.tools/problems/39e17e80-b382-4db5-8cdb-b4860b58141e).

# Research Brief: The Splay-Tree Deque Conjecture (ALGO-000088)

## What is the problem?

A **splay tree** (Sleator & Tarjan, 1985) is a binary search tree where every accessed or inserted/deleted element is moved to the root by "splaying." The **deque conjecture** asserts: any intermixed sequence of insertions and deletions at either end of a deque — i.e., operations `insert-min`, `insert-max`, `delete-min`, `delete-max` on the current item set — can be implemented on a splay tree in **O(n)** total time (excluding initial tree construction), for a sequence of n operations starting from an empty tree.

Formally: design the natural splay-tree implementations of the four deque primitives (e.g., insert-min splays the new node after hooking it in at the left spine; delete-min splays the minimum, then removes the root). The conjecture is that for every such sequence of n deque operations the total amortized cost — equivalently, the total splaying cost, since each splay's cost equals the length of the access path — is O(n). The claim is about the *amortized access lemma style* bound: no sequence of deque operations should cost more than linear total. Ambiguity note: the exact constant and whether one counts pointer rotations vs. depth are inessential; the open question is the O(n) total.

## Why does it matter?

The conjecture is one of the four classic splay conjectures (dynamic finger, working set, unified bound, deque/unified), and after the dynamic-finger and working-set conjectures were resolved, the deque conjecture remains a flagship test of Tarjan's claim that splaying is a universal self-adjusting optimum. A proof would complete a chapter of dynamic optimality theory and would likely require new lower-bound/charging techniques in extremal combinatorics (Davenport–Schinzel theory), with spillover to adaptive data-structure analysis generally. It also calibrates deques, the simplest non-trivial access pattern, as a benchmark for any dynamic-optimality candidate tree (e.g., multi-splay trees, zipper trees).

## What's already known?

- Sleator & Tarjan (1985) proved an O(n log n) bound trivially (each splay costs O(log n) amortized) and posed the conjecture.
- **Pettie (2008)**, the state of the art, proved a total bound of O(n α_3(n)) — essentially linear, where α_3 is a third-order inverse Ackermann–type function (a tower-of-logarithms iteration count). The proof recasts the total splaying cost as an extremal problem on **Davenport–Schinzel sequences** and bounds the number of "path-crossing" events. Getting from inverse-Ackermann overhead to exactly O(n) appears blocked by the limits of the DS-sequence machinery: the slack in the combinatorial bound is not obviously removable.
- Related resolved siblings: the dynamic-finger conjecture was proven by Cole et al. (~2000); the working-set bound by Sleator–Tarjan themselves. The full dynamic-optimality conjecture remains open, as does the traversal (scanning) conjecture in its general form (verify current status).

## What would count as solving it?

**Primary target (affirmative):** A proof that n deque operations on a splay tree take Θ(n) total time — i.e., total splay cost O(n) with an explicit constant — for the standard implementations of the four deque primitives.

**Negative resolution:** An explicit family of deque operation sequences forcing total cost ω(n) under any splay-based deque implementation (this would refute the conjecture and be equally publishable).

**Partial results of value:** improving Pettie's O(n α_3(n)) to O(n α(n)) or O(n · α_k(n)) with smaller k; resolving restricted variants (insert-only, delete-only, one-ended-with-peek, etc.); or extending the bound to other self-adjusting trees.

A referee would demand: a clearly specified implementation of the four operations, a well-defined cost model (number of rotations or path length per splay), and a rigorous amortized/charging argument valid for all adversarial sequences.

## How to start

1. **Read Pettie (2008) in full.** It is self-contained on the DS-sequence reduction. Understand exactly where the α_3 slack enters — identify the combinatorial inequality whose tight version would give O(n).
2. **Reproduce the Sleator–Tarjan potential argument** (Φ = log of sum of subtree weights) to see why standard potentials give only O(n log n); the known proofs for working-set/dynamic-finger fail for deque patterns because operations act at both ends, defeating monotone-weight potentials.
3. **Experiment:** write a splay-deque simulator, run random and structured adversarial sequences, and measure total cost / n. If superlinear behavior exists, a negative answer may be findable; empirical flatness suggests where a charging argument must live.
4. **Main obstacle:** the DS-sequence framework counts bad events coarsely; a linear bound likely needs a finer potential or a direct "threaded" argument tracking each item's left-to-right lifetime. Consider adapting Cole's dynamic-finger decomposition ideas.

## Key references

- Sleator, D. D., & Tarjan, R. E. (1985). "Self-Adjusting Binary Search Trees." *JACM* 32(3), 652–686. Defines splay trees, the potential method, the access lemma, and poses the deque, dynamic-finger, traversal, and dynamic-optimality conjectures. https://doi.org/10.1145/3828.3835
- Pettie, S. (2008). "Splay Trees, Davenport–Schinzel Sequences, and the Deque Conjecture." *Proc. 19th ACM–SIAM SODA*, 1115–1124. Proves the near-linear O(n α_3(n)) bound by reducing total splay cost to a Davenport–Schinzel-type extremal problem. (Also appeared in a journal version; verify.)
- Cole, R., Mishra, B., Schmidt, J., & Siegel, A. (2000). "On the Dynamic Finger Conjecture for Splay Trees, Part II." *STOC 2000*. Proves the dynamic-finger conjecture; the techniques (decomposing splay paths into small sub-splays) are the closest successful analogue. https://doi.org/10.1145/335305.335319

## Warnings

- The O(n) bound is *amortized over the whole sequence*, excluding initial tree construction — don't state it per-operation.
- Pin down the exact implementation conventions (how insert-min/max splice new nodes, how delete splays) before proving anything; sloppy conventions can change constants or even asymptotics of naive attempts.
- Don't re-derive the O(n log n) potential bound — it is trivial from the access lemma.
- Pettie's bound already looks "essentially linear"; purely cosmetic improvements to the inverse-Ackermann factor are not the goal — only exact O(n) or a lower bound resolves the conjecture.
- The census record is thin; verify all dates and the exact statement of Pettie's bound against the actual paper.

---
*`ALGO-000088` · unverified*

## Acceptance guidance

**Primary target (affirmative):** A proof that n deque operations on a splay tree take Θ(n) total time — i.e., total splay cost O(n) with an explicit constant — for the standard implementations of the four deque primitives.

**Negative resolution:** An explicit family of deque operation sequences forcing total cost ω(n) under any splay-based deque implementation (this would refute the conjecture and be equally publishable).

**Partial results of value:** improving Pettie's O(n α_3(n)) to O(n α(n)) or O(n · α_k(n)) with smaller k; resolving restricted variants (insert-only, delete-only, one-ended-with-peek, etc.); or extending the bound to other self-adjusting trees.

A referee would demand: a clearly specified implementation of the four operations, a well-defined cost model (number of rotations or path length per splay), and a rigorous amortized/charging argument valid for all adversarial sequences.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/39e17e80-b382-4db5-8cdb-b4860b58141e?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000088.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000088)
