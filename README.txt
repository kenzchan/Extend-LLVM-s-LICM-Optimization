HW2/                       # Your own project directory
      |-> build/                   # Build directory for your pass
      |-> CMakeLists.txt           # label as (a)
      |-> HW2/ 
               |-> CMakeLists.txt     # label as (b)
               |-> hw2.cpp         # Your LLVM pass

To make one should cmake .. under build directory and then make -j2
To run the benchmarks one should ./run.sh <benchmark name without the .c extension>

Our code implement following process:
  // 1. Classcify the basic blocks into freq and infreq basic blocks.
  // 2. Find store instrution in Infreq BB
  // 3. Find load and store in Freq BB and choose load in freq BB that olny have corresponding store in infreq BB but not freq BB
  // 4. reate a tempc (TEMP), for every same load operand(%i), 
  // 5. replaced every load instruction to a new load inst that takes in TEMP
  // 6. replcaed the orignal register (r10) to the new one (r100).
  // 7. Hoist the load inst
  // 8. Create a new store inst in preheader
  // 9. replaced the store inst in Infreq path to TEMP