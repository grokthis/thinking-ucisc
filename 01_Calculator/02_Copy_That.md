# Copy That!

I am tempted to say that the most important instruction in Micro-CISC is the
`copy` instruction, but you can't really do much without the `compute`
instruction as well. Since Micro-CISC only has 2 instructions, we will just say
that they are both important.

For now, however, we are going to focus on the `copy` instruction. The copy
instruction does exactly what it sounds like. It copies a value from one
location to another. You might be surprised how much time programs spend doing
this.

## How to Copy

To copy a value, you need to specify where you are copying the value from and
where you are copying the value to. Of course, you knew that. The important part
here is understanding what is a valid "from" and "to".

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

So, you can see, copy works by taking values from one place and putting them in
another. You can copy between memory locations and registers, memory to memory,
immediate values to registers and more. Any combination of "from" and "to" is
valid.

#### Immediate Values

Immediate values are numbers that are embeded in the instructions themselves.
Let's say you wanted to compare some number to zero and see if it was greater
than or less than zero. It's very convenient to be able to specify zero in an
immediate value rather than having to load it from memory somewhere. As we
explore Micro-CISC, you will find yourself using immediate values a lot.

In Micro-CISC, immediate values are numbers followed by `.imm`. Because of space
constraints in the instruction, these values must fall within certain value
ranges. We will cover these cases as we progress, but for now, you can rely on
the uCISC compiler to check the values for you.

Some examples:

```
0.imm
10.imm

# The answer to the ultimate question of life, the universe and everything:
42.imm

# The following is a hexadecimal value (i.e. "hex"). Hex values start with "0x"
# In this case, it is the hex value for 42.
0x2A.imm
```

Note that the `#` symbol denotes the start of a comment. These are human
readable text that the assembler skips when converting Micro-CISC into an
executable program.

#### Memory

All Micro-CISC programs operate on memory. You can think of memory as a big long
list of numbers, each of which has an address. The address of the first number
is 0, the second is 1, the third is 2, and so on. This is called a zero indexed
address since we start specifying the addresses with address 0. Zero indexing is
fairly common in computers.

We often don't want to directly know what location a value is stored at. We
often just get a reference to it's memory location so we can lookup the value.
Micro-CISC provides 3 of these references we can use: `1.mem`, `2.mem` and
`3.mem`. For example, the following instruction copies the value from location 1
to location 2:

```
copy 1.mem 2.mem
```

We can also copy immediate values into memory locations:

```
# Store 10 in the memory location referred to by register 2
copy 10.imm 2.mem

# Store 11 in the memory location referred to by register 3
copy 11.imm 3.mem
```

#### Registers

How does this magic work? How does the computer know what memory location is
referenced by `1.mem`? well, the computer has internal circuits to store
references called "registers". You can refer to the register for `1.mem` by the
name `1.reg`, for example. This means that register 1 holds the memory address
for memory value 1.

For example, when you write the following:

```
copy 10.imm 1.mem
```

The computer does the following:

1. Look at the value in register 1 (i.e. `1.reg`)
2. Pass that value to the memory controller as the address to lookup
3. Store the value 10 in that memory location

In summary, `1.reg` holds the memory address for `1.mem`. So when you want to
store a value at a memory location, you first need to load the address into it's
corresponding register.

```
# Copy address 20 into register 2
copy 20.imm 2.reg

# Store 5 at the address in 2.reg (which is address 20)
copy 5.imm 2.mem
```

## Sidebar on Hexadecimal

Hexadecimal numbers (or simply hex) is a numerical system with a base of 16
instead of 10 like we are used to dealing with. Hex numbers come up a lot in
programming, particularly in assembly because they are very convenient when
dealing with memory and bytes.

To understand why, consider the fact that computers operate on 1's and 0's. They
live in a base 2 world. Instead of the 10 digits (0-9) humans use, they get 2: 0
and 1. Place value works the same in base 2 as base 10. When you run out of
digits, you simply add one to the next value and reset the current place value
to 0.

In other words, `9 + 1 = 10`. With base 2 you just run out of digits sooner:
`1 + 1 = 0`.

Some examples of numbers and their binary equivalent:

| Base 10 | Base 2 (Binary) | Base 16 (Hex) |
|+------- |+--------------- |+------------- |
| 0       | 0               | 0             |
| 1       | 1               | 1             |
| 2       | 10              | 2             |
| 3       | 11              | 3             |
| 4       | 100             | 4             |
| 5       | 101             | 5             |
| 6       | 110             | 6             |
| 7       | 111             | 7             |
| 8       | 1000            | 8             |
| 9       | 1001            | 9             |
| 10      | 1010            | A             |
| 11      | 1011            | B             |
| 12      | 1100            | C             |
| 13      | 1101            | D             |
| 14      | 1110            | E             |
| 15      | 1111            | F             |

Notice that the first 16 numbers also represent the value 0000 to 1111 in
binary. This is why programmers tend to use hex a lot. Base 10 numbers don't
directly translate well to a specific number of bits in binary. Hex numbers do.
Every hex digit is 4 digits in binary. These 4 bits are called a "nibble" and
there are 2 nibbles in a byte and 2 bytes in a word.

So when you see the hex value `0xFFFF` you know that this is 4 nibbles, which is
2 bytes or 1 word. You an also immediately know that this number is all 1's in
binary. Since Micro-CISC processors are 16-bit processors, this is also the
maximum memory address that they can reference.

Now it's time to write some code.

## Copy That - Challenge

Let's copy some values around! Do the following:

1. Load 0xFFFF into register 1
2. Load 0x0000 into register 2
3. Copy the contents of memory address 0x0000 to memory address 0xFFFF

Hint: When you compile and run programs in Micro-CISC, they get loaded into
memory at address 0x0000.

Hint: Don't forget to halt your program as the last step with:

```
# Special halt instruction
copy 0.reg 0.reg
```

Hint: You can add data values to your program by adding lines that start with
"%" as follows (the values are in hex):

```
# The following adds 3 words of data to the program
% F0F0 0000 FFFF
```

You can find one possible answer to this challenge in
[examples/copy_that.ucisc](examples/copy_that.ucisc).
