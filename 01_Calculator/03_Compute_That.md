# Compute That!

Now for the other Micro-CISC instruction, `compute`. Often, you will want to
modify the value you are moving from one place to another. Perhaps you want to
add or multiply the values along the way. That is what compute is for.

## How to Compute

Here is the cool thing about Micro-CISC. The from and to arguments can have
exactly the same values as before.

The "from" value can be one of the following:

* An immediate value, for example: 0.imm or 10.imm
* One of the 3 general registers, for example: 1.reg or 2.reg
* One of the 3 memory locations, for example: 1.mem or 2.mem
* The program counter register: 0.reg

The "to" value can be one of the following:

* One of the 3 general registers, for example: 1.reg or 2.reg
* One of the 3 memory locations, for example: 1.mem or 2.mem
* The program counter register: 0.reg
* A special "control" register, which we will ignore for now.

## Computing Results

Compute instructions need an "op code" to tell it what math operation it is
computing. To do this, it trades bits that were previously dedicated to
immediate value and instead uses those bits for an ALU op code. Note that some
computations take two arguments. In that case, both the source and destination
of the instruction are used as inputs to the computation. The result is stored
in the destination, overwriting whatever was there before.

You can find the
[full list of opcodes](https://github.com/grokthis/ucisc/blob/master/docs/09_Instruction_Set.md#a-compute-opcode)
on the main uCISC repo, but we will cover a few of them here for our purposes.

* `0xA.op` - Add: adds source into destination
* `0xB.op` - Subtract: subtracts source from destination
* `0xC.op` - Multiply: multiplies source into destination
* `0xD.op` - Divide: divides source into destination

*Note:* By default, Micro-CISC processors are in signed mode. All values are interpreted
as 2's compliment signed numbers. This means 16-bit values can only contain
values between -32,768 and 32,767 by default. If a computation results in a
value outside of this range, an overflow will occur. For now, we will keep our
values within the necessary range and deal with overflows in a bit.

Now that we have both copy and compute, we can start to do some more interesting
things in our challenges.

## Compute That - Challenge

We can start to put together something that starts to look like a calculator.

1. Load 0xFFFB into register 1
2. Solve the following 5 math problems and put the answer in the specified
   result memory address.
  * 54 + 62 = ?; store in 0xFFFB
  * 54 * 36 = ?; store in 0xFFFC
  * 24 / 12 = ?; store in 0xFFFD
  * 22 - 12 = ?; store in 0xFFFE
  * 12 / 5 = ?; store in 0xFFFF

The stack results for this program should be as follows:
```
INFO: Stack: FFFB => 0x0074 0x0798 0x0002 0x000A 0x0002 
```

*Bonus:*

What happened to remainder in the last problem? Add it to the stack at address
0xFFFA if you can. You'll need to dig into the specifics of the divide opcode in
the compute documentation.
[Full list of opcodes is here.](https://github.com/grokthis/ucisc/blob/master/docs/09_Instruction_Set.md#a-compute-opcode)

You can find one possible answer to this challenge in
[examples/compute_that.ucisc](examples/compute_that.ucisc).
