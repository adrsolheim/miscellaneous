All assembly code is just numbers, even the `mov` instructions (`mov` just points to a fixed unique number.) CPU registers are global variables (`rax`, `rsi`, `eax`) and there are 8-32 of them. On a 64-bit machine the registers can store up to 64-bit numbers. If you need more, you need to use more than one variable.
The program counter (`pc`, `eip`, etc) is a special registers which tells the CPU which instruction to execute next. 
Assembly is like a sheet of intructions from top to bottom where each has an address (`0x4005db`) and the program counter will point to the next address in the program. Like a programming language, assembly fetches values from memory and puts them into registers (like variables), does something with those variables, and returns the result to memory. Registers are much faster than memory, but space is limited so we need to utilize memory. If some memory addresses keep on being reused the CPU can store them in CPU cache which is slower than registers, but faster than memory.
```
0x4005db mov rax, rsi
0x4045db add rbx, 0x1
0x4085db test rbx, rbx
0x4105db mov rax, rsi
...
```
Assign 5 to register, add 3.
Assign 8 to another register.
Subtract the first register from the second register

```
mov eax, 0x5
add eax, 0x3
mov ebx, 0x8
sub eax, ebx
```
`eax` now holds 0.

What doesn't fit in registers lives in memory (using an address to store a value instead of an instruction?)

Accessing memory with loads and stores at addresses (like an array of memory)
```
0x00000000 0x1234
0x00000005 mov eax, [0x00000000]
```
Accessing memory through `PUSH` and `POP` operations on a stack. `sp`, `esp`, or `rsp` point to the top of the stack. If `esp = 20` then the top of the stack is at address 20.
```
0x00000005 push 0x5
0x00000010 pop eax 
```
`eax` now holds 5.

**Control flow** is done via `GOTO`s (`jump`, `branch`, `call`)
```
0x0000ac mov rax, rsi
...
0x4005db jmp 0x0000ac	; mov eip, 0x0000ac 
```
**Status flags** says something about the resulting effects of previous instructions (e.g. last value computed to zero -> zeroflag = 1) 
Here it jumps to address 0x000 because the flags set following the subtraction instruction tells `jl` that "Yes, eax < ebx"
```
mov eax, 0x5
mov ebx, 0x8
sub eax, ebx
jl 0x000
```
`call` pushes the next address on the stack, and `ret` pops it off and keeps going where the `call` instruction was execution (this acts like a regular function).
