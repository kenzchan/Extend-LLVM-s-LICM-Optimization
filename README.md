# Extend-LLVM-s-LICM-Optimization
The goal of this project is to extend the LLVM loop invariant code motion (LICM) optimization to identify more opportunities for optimization using control flow profile information. 
In traditional LICM, the instructions that are invariant along the most likely path through the loop will not be hoisted because traditional LICM must ensure that execution is correct regardless of the path taken through the loop body.
However, by considering just a single path, we can be more aggressive and hoist additional instructions.
This optimization is a form of compile time speculation in that we are guessing the frequent path is followed. Whenever another path is taken, we must handle the mis-speculation and ensure correct execution. With LICM, mis-speculation handling can be accomplished by simply redoing the code that was speculatively hoisted.

# Implementation
Extend LLVM’s LICM (see llvm-source-dir/lib/Transforms/Scalar/LICM.cpp) to perform speculative hoisting of “almost invariant” instructions. Almost invariant is defined as having a single source operand that is invariant along the most likely path through the loop but variant along 1 or more other, infrequent paths (the other source operand is completely invariant). As a simplifying assumption, you do not need to worry about pointers in this optimization. Rather, almost invariant instructions will be detected through explicit modification of a source operand from an infrequent block but invariant in the frequent blocks of the loop. Whenever the compiler speculates, it must have a repair mechanism to handle mis-speculations. With LICM, the hoisted instruction can simply be re-executed whenever an infrequent path is taken. Your implementation should consist of 4 parts:
1. Identify most likely path through a loop body. We will focus on innermost loops only. This can be accomplished by starting at the loop header and repeatedly following the most likely branch until a likely loop backedge is taken. All blocks along this path are classified as frequent and the rest in the loop body as infrequent.
2. Identify almost invariant instructions among the frequent blocks.
3. Create a heuristic to decide if hoisting the almost invariant instruction is profitable or not, and
apply hoisting to profitable opportunities. Profit can be estimated by counting the number of the
estimated dynamic instructions with and without hoisting.
4. Create and insert the repair code in case of mis-speculation.
