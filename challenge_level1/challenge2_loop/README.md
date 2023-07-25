## README - Fixing errors in the testbench RISC-V assembly code

**Problem Description:**

The code is intended to perform certain tests, but due to errors in the code, it got stuck in an infinite loop, preventing the test from completing successfully.

**Solution Steps:**

1. **Understanding the RISC-V Testing Framework:**

   - The RISC-V Testing Framework is a set of assembly code files that are used to test the functionality of the RISC-V processor. The framework is used to test the processor at the RTL level, and it is also used to test the processor after it is synthesized and implemented on an FPGA.

2. **Understanding the Testbench Assembly Code:**

    - Looking at the testbench assembly code at `test.S`, I noticed in the `loop:` label that there was a `beq` branch instruction that branched to the `loop` label itself. This caused the code to get stuck in an infinite loop, preventing the test from completing successfully.

    - In the setup of the testbench, a register is loaded with the total number of tests that are going to be run. This counter is not utilized in the loop label, and hence the loop is not terminated.

3. **Fixing the Error:**

    - I corrected the loop by changing the `beq` instruction to a `bne` instruction. This caused the loop to branch to the `fail` label if the compared registers are unequal.

    - I added an addi instruction to decrement the counter register by 1 in each iteration of the loop. This ensures that the loop terminates after the specified number of tests are run.

4. **Result:**

The code successfully completed the test after the above corrections.

```
loop:
  lw t1, (t0)
  lw t2, 4(t0)
  lw t3, 8(t0)
  add t4, t1, t2
  addi t0, t0, 12
  bne t3, t4, fail        # check if sum is correct

  addi t5, t5, -1

  bne t5, x0, loop      # If t5 != 0, continue looping (jump to 'loop')

  j test_end
```

**Conclusion:**

By understanding the RISC-V Testing Framework, analyzing the code, and making the necessary corrections, I successfully fixed the errors in the testbench assembly code.