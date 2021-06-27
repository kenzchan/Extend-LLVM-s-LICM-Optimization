# Extend-LLVM-s-LICM-Optimization
The goal of this project is to extend the LLVM loop invariant code motion (LICM) optimization to identify more opportunities for optimization using control flow profile information. 
In traditional LICM, the instructions that are invariant along the most likely path through the loop will not be hoisted because traditional LICM must ensure that execution is correct regardless of the path taken through the loop body.
However, by considering just a single path, we can be more aggressive and hoist additional instructions.
This optimization is a form of compile time speculation in that we are guessing the frequent path is followed. Whenever another path is taken, we must handle the mis-speculation and ensure correct execution. With LICM, mis-speculation handling can be accomplished by simply redoing the code that was speculatively hoisted.

# Implementation
Our code implement following process:
* Classcify the basic blocks into freq and infreq basic blocks.
* Find store instrution in Infreq BB
* Find load and store in Freq BB and choose load in freq BB that olny have corresponding store in infreq BB but not freq BB
* Create a tempc (TEMP), for every same load operand(%i), 
* Replaced every load instruction to a new load inst that takes in TEMP
* Replcaed the orignal register (r10) to the new one (r100).
* Hoist the load inst
* Create a new store inst in preheader
* Replaced the store inst in Infreq path to TEMP

# Directory
      |-> build/                   # Build directory for your pass
      |-> CMakeLists.txt           # label as (a)
      |-> HW2/ 
               |-> CMakeLists.txt     # label as (b)
               |-> hw2.cpp         # Your LLVM pass

To make one should cmake .. under build directory and then make -j2.
To run the benchmarks one should ./run.sh <benchmark name without the .c extension>
